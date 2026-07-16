# SattvaLand — Project Memory

## What this project is

A land-intelligence portal for Karnataka (Bangalore Urban, Bangalore Rural, Mysore), modeled on an existing Delhi NCR portal. Starting from a **Google Maps pin**, the system must automatically pull, cross-verify, and overlay government land records, zoning, and village-map data onto a map, score each parcel, and monitor sources 24/7 for changes.

**The single source of truth for scope is `docs/action-plan.md`** — the action list distilled from the kickoff call (2026-07-16) and the client's summary email. Read it before making product or architecture decisions; nothing in it should be silently dropped.

## Data sources (Karnataka)

| Source | Role | Input → Output | Friction |
|--------|------|----------------|----------|
| **Dishaank** (app-only, Survey Dept.) | Entry point | Lat/long → survey number + present owner | No website; need underlying API |
| **Bhoomi** (Revenue Dept.) | Authoritative ownership | District/taluk/hobli/village + survey no. → RTC owner, disputes/court stays | Login + OTP |
| **Kaveri** | Transaction history | Survey no. / boundaries / owner name → EC (chronological registered transactions, document numbers); then auto-apply for Certified Copy (CC) of a document | Login; govt fee payment; CC approval takes ~3 days |
| **CDP / Master Plans** | Zoning overlay | Zone polygons (yellow/red/blue/purple/orange…) | PDF/scan; needs georeferencing; Landeed benchmark ~80% accuracy, target ~100% |
| **e-Chavadi** | Village map overlay | Village map with survey numbers + features (nala, lake, graveyard, government land) | Old (1904/1908-era) scanned maps; feature extraction |

## Key domain facts (do not lose these)

- **Survey number is the join key** across all sources; the Google pin → survey number resolution (Dishaank) is the root of the whole pipeline.
- Dishaank and Bhoomi are run by **different departments and can disagree** — always cross-compare ownership across Dishaank, Bhoomi, and Kaveri CC, and **highlight discrepancies**.
- **Land conversion (agricultural → non-agricultural) is NOT reflected in Bhoomi/Dishaank** — it only appears in the sale deed / certified copy. Must be extracted from deed documents.
- Village-map features near a parcel (~100 m): **nalas (NGT buffer rules), lakes/water bodies (even dried), graveyards (smashana), adjacent government land** — all must surface in parcel details and feed the score.
- Growth scores are **dynamic** (news/notifications can move a score overnight) and the scoring weights are **tweakable per client**.
- Some portal content is in **Kannada**; some flows need **payment (recharge wallet is acceptable — cost is not a concern)**.
- Parcel workflow: favorites, **rejection tracking with reasons**, full worked-parcel history, periodic (~3-month) revisit of rejections.

## Conventions

- Repo was empty at project start; this file and `docs/action-plan.md` are the founding documents.
- Update `docs/action-plan.md` (feasibility statuses, open questions) as decisions land — it doubles as the running checklist for the weekly Friday client calls.
