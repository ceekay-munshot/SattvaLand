# F6 — Land conversion (agricultural → non-agricultural) status

**Key fact (do not lose):** conversion is **NOT reliably reflected in Bhoomi (RTC) or Dishaank** — the exact gap the client stressed on the kickoff call. It lives in the **DC conversion order** and, when the land has since been sold, in the **sale deed**.

## Where the conversion data actually comes from — ranked (easiest first)

1. **Sale deed + EC (Kaveri) — primary, and easiest to automate.** A post-conversion sale deed cites the **DC conversion order number and year**; the EC transaction chain references it too. Since F4 (EC) and F5 (certified deed) already pull these, F6 extracts conversion status from them with document AI — no extra source needed. **This is F6's primary route.**
2. **Seller / current owner — easiest in a live deal.** The owner physically holds the conversion order. Caveat (lawyers stress this): **never accept a photocopy as proof — require a certified copy.**
3. **RTC / mutation records (Bhoomi) — free digital cross-check.** After conversion, the RTC classification is *supposed* to be updated via a Taluk-office mutation (agri → converted), with a mutation (MR) entry referencing the order. In practice this is **inconsistently updated** (the client's point), but when present it's a free cross-check; the village accountant's (VA) records hold the detail.
4. **DC / Tahsildar / Nadakacheri office — authoritative fallback, traceable from the survey number.** Important practical point: **you do not need the order number** — the survey number + village + hobli is enough for the office to trace the file and issue a **certified copy**. Slower / semi-manual, but authoritative.

## 2025 context (changing, but no clean survey-number lookup yet)

- Karnataka moved DC conversion **applications** onto the **DC e-Office** online system under the Sakala framework.
- The **Karnataka Land Revenue Rules Amendment 2025** added **"deemed approval"** if the DC does not act within 30 days.
- So *new* conversions leave a digital trail — but there is still **no clean "survey number → conversion order number" public portal** for historical conversions. The RTC (after mutation) is where it is meant to surface.

## Implication for the product / proposal

F6's primary route should read: **"extract conversion status from the certified sale deed / EC (Kaveri); RTC mutation on Bhoomi as a free cross-check; DC/Tahsildar office as the authoritative fallback, traceable from just the survey number."**

Earlier proposal wording ("search the Karnataka Land Conversion portal" as the *primary* route) is over-optimistic — correct it to the deed-first phrasing above.

## Sources

- PKP Advocates — DC Conversion Order in Karnataka: how to get a copy and read it: https://www.pkpadvocates.in/blog/agricultural-land-conversion-bangalore-dc
- Cyril Amarchand — Karnataka Land Revenue Rules Amendment 2025 (deemed approval, DC e-Office): https://corporate.cyrilamarchandblogs.com/2025/11/karnataka-land-revenue-rules-amendment-2025-redefining-land-governance/
- OneCity — Agricultural land conversion in Karnataka (process & fees): https://www.onecityproperty.com/news/guide-to-agricultural-land-conversion-in-karnataka-process-fees-and-legal-requirements
