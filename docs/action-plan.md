# SattvaLand Bangalore — Action Plan

Distilled from the kickoff call transcript (2026-07-16) and the client's summary email. This is the guidance document: **nothing here should be silently dropped**. Feasibility items are to be tackled one by one; update statuses inline.

---

## A. The core product flow (what the portal must do, step by step)

The end-to-end pipeline, translating the client team's manual workflow into system steps:

1. **Google Pin input** — The entire flow starts with a Google Maps pin (longitude/latitude), *not* a survey number. Brokers share Google pins; that is the entry point.
2. **Pin → Survey Number (Dishaank)** — Resolve lat/long to the survey number and present ownership, replicating what the field team does on-site with the Dishaank app. Dishaank is app-only (iOS/Android), no website. Dishaank also provides a satellite image of the parcel (today used for manual matching against Google Maps, ~5–10 min — to be automated).
3. **Ownership verification (Bhoomi)** — Using district / taluk / hobli / village / survey number, pull the RTC (present owner) from Bhoomi. Bhoomi (Revenue Dept.) is more reliable than Dishaank (Survey Dept.); the two can be out of sync. **Cross-compare both and flag mismatches.**
4. **Dispute / litigation check (Bhoomi)** — Bhoomi shows court stays/disputes on a property; this alone covers a large share of the legal check. Login is OTP-based; anyone can register.
5. **Encumbrance Certificate pull (Kaveri)** — Run an EC search by the property's **survey number, boundaries, or owner's name**. The EC displays a **chronological table of all registered transactions** (last ~14 years, 2012 onward). Locate the relevant transaction (e.g., the last sale deed); its **Document Number and registration year** are listed alongside (e.g., 2480/2016-17).
6. **Certified Copy retrieval (Kaveri)** — **Auto-apply for the CC (certified copy)** of that Document Number for 100%-accurate owner information. Today this takes ~3 days (official fee + sub-registrar approval). Goal: automate the application + payment and retrieval; a recharge/wallet facility for government fees is acceptable — cost is not a concern.
7. **Conversion status detection** — Key data gap: agricultural → non-agricultural conversion is **not** reflected in Bhoomi/Dishaank. It only appears in the sale deed / certified copy ("this is a converted land"). Extract conversion status from deed documents.
8. **Owner cross-check across all sources** — Compare owner details from Dishaank, Bhoomi, and the Kaveri CC (and the broker's claim) and **highlight discrepancies** prominently.
9. **CDP / Master Plan zone overlay** — Superimpose CDP zoning (yellow, red, blue, purple, orange, green belts, etc.) onto Google Maps. Benchmark: the Landeed app achieved ~80% accuracy; target ~100%. The zone must (a) show on hover/click of a parcel and (b) **feed the scoring model** (e.g., red zone = public use, very hard to convert → lower score, longer timeline).
10. **e-Chavadi village map overlay** — Superimpose the village maps (1904/1908-era, still the "bible" for Karnataka land) onto Google Maps. Auto-detect overlapping and nearby structures (~100 m radius): **nalas** (buffer + NGT rules), **lakes/water bodies** (even dried ones), **graveyards (smashana)**, and **adjacent government land** (e.g., the 43/1 example — risk of slum/public construction). These flags surface in parcel details and **feed the scoring model**.
11. **Growth score engine** — Like the Delhi portal: a tweakable algorithm scoring each parcel on ownership history, proximity signals (airport/highway), CDP zone, village-map risk features, disputes, and live news/notifications. Scores are **dynamic** — a 90 can drop to 50 when new information arrives.
12. **24/7 monitoring + notifications** — AI agents continuously watch all wired sources (government portals, news, podcasts, scheme notifications, government announcements) and push alerts when anything changes on tracked/favorited parcels. "I should be the first one to know."
13. **Parcel workflow features** — Favorite parcels; **rejection tracking** (mark rejected with the reason, e.g., "CDP zone issue"); full history of all parcels worked (e.g., 50 worked → 40 rejected, 10 in discussion); periodic (~every 3 months) revisit of rejections since circumstances change.
14. **Coverage** — **Bangalore Urban, Bangalore Rural, Mysore** (confirmed in email). All agreed on the call to narrow to specific taluks/sub-districts first for faster value — **Vivek to specify the priority pockets** (still open).

## B. Feasibility checks (immediate work, due before the Friday client call)

For each source: can we programmatically fetch it, what are the inputs/outputs, and what are the blockers (login, OTP, CAPTCHA, payment, app-only, Kannada)?

| # | Source | What we pull | Known friction to test | Status |
|---|--------|--------------|------------------------|--------|
| F1 | **Dishaank** (app-only) | Lat/long → survey number + present owner + satellite view | No website; need its underlying API/endpoint or an SSLR equivalent | ✅ **Partially feasible** — see [`feasibility/F1-dishaank.md`](feasibility/F1-dishaank.md). Survey-number half works via KSRSAC K-GIS `getlocationdetails` API; owner half is F2 (Bhoomi). Caveats: India-only egress + likely non-commercial/whitelisted terms. Next: India-egress spike on 20–30 pins + email KSRSAC. |
| F2 | **Bhoomi** | Survey number → RTC/owner, dispute/stay entries | Login + OTP; multi-field input (district/taluk/hobli/village) | ☐ |
| F3 | **Kaveri** | EC search (by survey no. / boundaries / owner name) → transaction table + document numbers; auto-apply for CC | Login; government fee payment; ~3-day sub-registrar approval on CCs — test how fast retrieval can really be | ☐ |
| F4 | **CDP / Master Plans** | Zone polygons (Bangalore Urban first) | PDF/scan from the authority site; georeferencing onto Google Maps at high accuracy (beat Landeed's ~80%) | ☐ |
| F5 | **e-Chavadi** | Village maps with survey numbers + features (nala, lake, graveyard, govt land) | Old scanned maps; feature extraction; overlay alignment | ☐ |
| F6 | **Language** | — | Portal content partly in Kannada — verify reliable translation/normalization to English | ☐ |
| F7 | **Payments** | — | Verify a recharge/wallet flow works programmatically for Kaveri fees | ☐ |
| F8 | **Cross-source join** | — | Survey number as the common key across Dishaank–Bhoomi–Kaveri–CDP–e-Chavadi; pin→survey accuracy good enough to replace today's manual satellite matching | ☐ |

**Early risk flags:**
- The **3-day certified-copy turnaround is a government process**, not a scraping problem — we can automate application, payment, and retrieval, but likely cannot compress the sub-registrar's approval itself.
- **Automated pulls from OTP/login-gated government portals** need a workable (and acceptable) authentication approach — whose accounts, how OTPs are handled.

## C. Project & business actions

1. ✅ **Summary email received** from Sahil (Dishaank → Bhoomi → Kaveri flow, cross-check, Master Plans, e-Chavadi, regions).
2. ☐ **Run feasibility checks F1–F8** and report at Friday's call.
3. ✅ **WhatsApp group** set up on the call; confirm Vivek's availability for Friday ~2:00–2:30 PM.
4. ✅ **Weekly recurring Friday calls** — invite sent.
5. ☐ **Bangalore visit** — week after next; meet Vivek's team (important given the ongoing transition); confirm dates. Portal need not be ready by then.
6. ☐ **Commercials** — Sahil is the single point of contact for documentation/contract; discuss after feasibility is confirmed.
7. ☐ **3-month paid pilot** — dedicated AI engineer, dashboard build, monitoring agents, infra + token costs included; unlimited dashboards; client keeps or drops after 3 months.

## D. Open questions

1. Which specific taluks/pockets in Bangalore Urban/Rural and Mysore to start with (Vivek).
2. ~~Village-map portal name~~ — **resolved: e-Chavadi** (per summary email).
3. CDP source of truth — which authority's map/version is canonical; is Mysore's master plan in scope too (call discussed Bangalore Urban's CDP)?
4. Credentials strategy — whose Bhoomi/Kaveri accounts we operate under; who funds the recharge wallet.
5. Scoring weights starting point — how the Bangalore algorithm should differ from Delhi's (zone-heavy vs proximity-heavy); tweakable, but needs a v1.

## E. Items from the call NOT captured in the summary email

Kept here so they don't get lost (consider replying to the email to append them):

- **Conversion status extraction** from sale deeds (the Bhoomi/Dishaank blind spot Vivek stressed).
- **Dispute/court-stay check** via Bhoomi as part of the legal screen.
- **Dynamic growth scoring + 24/7 monitoring/notifications** (the Delhi-portal behavior to replicate).
- **Favorites + rejection tracking with reasons + ~3-month rejection revisits + worked-parcel history.**
- **Narrowing focus areas** within the three districts for faster time-to-value.
- **Kannada language handling** and **payment/recharge wallet** requirements.
- **Google pin as the mandatory entry point** (email lists it as Dishaank input, but the call was explicit: never start from a survey number).
- Proximity/distance display on parcel hover (airport, highway, key structures) as in the Delhi portal.

## F. Data-sourcing strategy — Landeed

**What Landeed is:** a broad, Karnataka-covering land-records provider — EC, RTC, sale deeds, AI title reports, encumbrance monitoring across 20+ states — with a public **"Document Fetch API"** and a **BDA CDP / land-use viewer** (pin/address → zone R/C/I/PSP/OS/AG, RMP 2015 + draft RMP 2031, "sourced from BDA master-plan maps and official GIS layers"). B2B/enterprise offered; freemium then per-document / enterprise-via-sales. It is the ~80%-accurate CDP benchmark Vivek referenced on the call (he asked Landeed to build it, they did ~80%, then stopped). Note: its CDP tool is pin → **zone**, not pin → **survey number** — it does **not** solve F1.

**Decision (2026-07-16, tech@muns.io):** Do **NOT** license Landeed's API. Instead obtain the data by **scraping Landeed's public web tool / URLs** (e.g. `web.landeed.com/karnataka/bangalore-development-authority-bda-land-use`). Rationale given: the URL is publicly accessible.

**Recorded caveats / risks (flagged; the decision stands as a muns.io/client business call):**
- Landeed's CDP zoning layer is their **derived, hand-digitized work product**, not raw public data. Scraping it copies their database → terms-of-use violation and potential copyright / database-right exposure. This is a legal-risk decision for muns.io / counsel — "public URL" ≠ "free to reuse."
- **Relationship risk:** Landeed is a peer Vivek personally knows and asked to build this. Discovery could damage the client relationship.
- **Accuracy self-defeat (CDP):** scraping Landeed's CDP inherits their ~80% and their errors — the exact ceiling Vivek is dissatisfied with — which contradicts the ~100%-accuracy differentiator. For CDP, digitizing the **official BDA / RMP-2031 source** is both cleaner (no third-party IP) and the only path to beat 80%.
- **Middleman inefficiency (land records):** for EC/RTC/deeds, Landeed is itself just fetching government documents. Going direct to Bhoomi / Kaveri / KGIS (already the plan) is more direct than scraping a middleman.

**Recommended mitigation (to revisit):** prefer official government sources for CDP (BDA / RMP-2031) and land records (Bhoomi / Kaveri / KGIS); use Landeed only where no official route is viable, and keep the licensing option open as a fallback.
