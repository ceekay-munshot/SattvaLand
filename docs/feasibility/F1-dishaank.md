# F1 — Dishaank: pin (lat/long) → survey number + owner

**Verdict: PARTIALLY FEASIBLE** (medium confidence). Researched 2026-07-16 via a verified web-research sweep (5 angles, 12 load-bearing claims adversarially fact-checked; 9 held, 3 refuted).

## Bottom line

- The **survey-number half is feasible today** via a documented Karnataka government API. Dishaank (`com.ksrsac.sslr`) has no public API of its own, but it is a client of **KSRSAC's K-GIS "Generic Web Services"**, which are openly documented and confirmed in live use by 6+ independent GitHub projects.
- The **present-owner half is NOT part of F1.** No geo/coordinate endpoint returns an owner. Owner comes only from a second-stage, per-parcel **Bhoomi RTC** lookup (CAPTCHA/OTP/paid) keyed by the survey number — i.e. it belongs to **F2**. There is no lawful bulk lat/long → owner path.

> Nuance: the Dishaank *app* does show the owner to a user, but only by linking out to the Bhoomi RTC after it identifies the survey number. So "Dishaank gives owner" is true at the app level and false at the API level. Plan the owner step as F2.

## Primary route (verified) — KGIS `getlocationdetails` Web API

```
GET https://kgis.ksrsac.in:9000/genericwebservices/ws/getlocationdetails?coordinates=<lat>,<lon>&type=dd
```
- Pass the raw Google WGS84 lat/long (`type=dd`, no reprojection).
- Read `data[0]` after checking `message == '200'`.
- **Rural points** return `districtName / talukName / hobliName / villageName` (+ LGD codes) and **`surveynum`**.
- **Urban points** return only BBMP/town **ward + zone** — *no survey number* (agricultural survey numbers exist only on rural/revenue land).
- Optionally chain the village code into the `/surveyno` variant with a distance buffer to list adjacent parcels near a boundary (gives survey + hissa).
- **Effort: low.** Endpoint existence and request/response shape are verified.
- Docs: https://kgis.ksrsac.in/kgis/webapi.aspx (mirror: https://ksrsac.karnataka.gov.in/kgis/webapi.aspx)

### The two real caveats on this route (must clear before building)
1. **Geofencing.** The KGIS host (both `:443` and `:9000`) returns 503 / TCP-reset to non-India / datacenter IPs. It must be called from an **Indian egress IP** (e.g. Mumbai / `ap-south-1`, or an India VPS). GitHub code proves the endpoint's structure — not that it serves arbitrary global callers.
2. **Access + licensing gating** (real, but low-confidence on specifics). The Web API is likely **whitelisted to government departments/partners**, and KGIS data is stated **free / non-commercial**. Anonymous production access is *not* confirmed, and a commercial SattvaLand product may breach the non-commercial condition. **Must clarify with KSRSAC** (`kgissupport@ksrsac.in` / `ksrsac-gok@karnataka.gov.in`).

## Fallback routes

| Route | How | Gives | Effort | Note |
|-------|-----|-------|--------|------|
| **Buy parcel data (TalkingLands / Landeed)** | License a Karnataka survey-number parcel-polygon layer → offline point-in-polygon (`ST_Contains` in PostGIS) on the pin. Landeed also does survey → owner/area. | Survey polygon (offline PIP); Landeed adds owner direction | medium | Coverage/accuracy vary by village — spot-check a sample. Pricing sales-gated. Landeed's documented direction is survey→lat/long, not a documented reverse lat/long→survey. Medium confidence, not independently verified. |
| **KSRSAC cadastral data MOU → offline PIP** | Obtain the cadastral layer under a data agreement, load into PostGIS, PIP against the pin. Removes runtime dependence on the geofenced API. | Survey polygons (offline/bulk), no owner | high | Slow/bureaucratic; grant to a commercial entity uncertain; likely same non-commercial condition. **No free official bulk download exists** (public KGIS downloads are admin boundaries only). |
| **KGIS ArcGIS REST query on a cadastral MapServer** | POST a point to a queryable cadastral MapServer / WMS GetFeatureInfo to read the survey-number attribute. | Survey-number attributes — *if* such a service is published | medium | **WEAK / do not build on this.** The only confirmed cadastral service (`CadastralCache_State`) is a cached tile service with no per-parcel attributes. Specific attribute-bearing service names from research were **refuted (partly fabricated)**. |

## Owner second stage (this is F2, flagged here for continuity)

With survey/hissa + admin hierarchy known, get owner from the **Bhoomi RTC**: per-parcel view at `landrecords.karnataka.gov.in` (CAPTCHA), signed i-RTC at `rtc.karnataka.gov.in` (~₹10, OTP/login), or a commercial reseller (SurePass "Karnataka Land Record Check", Landeed). **No official bulk/automated owner API exists** — API Setu's Karnataka land-records API is *verification-only* (needs a document number you already hold). Bulk scraping that defeats CAPTCHA/OTP implicates IT Act 2000 s.43/s.66, and owner names are personal data under the DPDP Act 2023.

## Accuracy ceiling (set client expectations)

Cadastral-map-to-satellite georeferencing offset + 3–10 m GPS error stack. Output is **advisory only**; screenshots are explicitly not court-admissible. The manual "match Dishaank satellite image to Google image" step the team does today cannot be fully automated away.

## Recommended next action (the F1 spike)

1. **Empirical spike from an India egress** (Mumbai/`ap-south-1` cloud or a cheap India VPS): call `getlocationdetails?coordinates=<lat>,<lon>&type=dd` against **20–30 known pins** across target districts (mix rural + urban) and measure: (a) does it respond 200 anonymously or need whitelisting; (b) survey-number correctness vs Dishaank on the same pins; (c) urban behavior (ward/zone vs survey).
2. **Email KSRSAC** (`kgissupport@ksrsac.in`) to clarify access terms + commercial-use eligibility.
3. **Request coverage samples + quotes** from TalkingLands and Landeed as the buy-don't-build fallback.
4. **Scope present-owner as a separate F2 workstream** — do not assume it comes free with the pin.
5. **Do NOT build** on the `/kgissurveynumber` chain or the specific ArcGIS Tank/Forest/Govt_Land GeoJSON service names — both were adversarially found to be partly fabricated.

## Open questions

- Is `getlocationdetails` callable anonymously from an Indian IP, or does KSRSAC require whitelisting / a token / an MOU for production volume?
- Does our use case count as "commercial" under the KGIS free/non-commercial condition, and what licensing would KSRSAC then require?
- Real survey-number hit rate/correctness across target districts, and how urban/BBMP pins (no survey number) are handled?
- TalkingLands' and Landeed's actual Karnataka coverage, accuracy, pricing, and whether either exposes a true lat/long→survey reverse lookup?
- Does the shipped Dishaank app hit `:443` (per a Play Store error report) rather than the documented `:9000` — i.e. a different/internal endpoint? (medium-confidence gap; no teardown exists)
- For the owner half: what monthly volume is needed, and does per-parcel Bhoomi / a reseller work economically and legally at that scale under DPDP?

## Reference URLs

- K-GIS Web API list: https://kgis.ksrsac.in/kgis/webapi.aspx
- KGIS downloads (admin boundaries only): https://kgis.ksrsac.in/kgis/downloads.aspx
- Confirmed cadastral tile service (cached, no attributes): https://kgis.ksrsac.in/kgismaps1/rest/services/Cadastral/CadastralCache_State/MapServer
- API Setu Karnataka land records (verification-only): https://directory.apisetu.gov.in/api-collection/landrecordskar
- Android app: `com.ksrsac.sslr` · iOS app id: `6741137784`
