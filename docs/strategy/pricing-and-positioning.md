# Munshot — Pricing & Positioning Strategy

Working strategy notes for how Munshot prices and positions the SattvaLand engagement (and future enterprise engagements). Distilled from the pricing discussion; treat as living.

> Naming note: the vendor/platform is written as **"Munshot"** (matches muns.io and all proposal drafts). The client once wrote "Moonshot platform" — open question whether the platform carries a distinct name. Resolve before finalising client-facing docs.

## Positioning — what Munshot actually is

Not a dashboard project. Munshot is the **AI operating layer of an enterprise**, entering through the division with the sharpest pain (here, **Land**), then expanding division by division (Land → Sales → Marketing → Legal → Finance → Procurement → HR). The value sold is **labour, not software** ("digital employees" / services-as-software).

One-liner: **"Palantir's go-to-market (embed in one division → expand across the org), executing the services-as-software thesis, for traditional Indian enterprises."**

Methodology framing (client-facing): **We are your AI team — we diagnose, build, and deploy onto the Munshot platform.**

## Closest comparables (and what to borrow)

| Comparable | What they are | What to steal |
|---|---|---|
| **Palantir** | Forward-deployed, land-and-expand | The *motion*: paid pilot → expand across divisions; value = account ACV, not the first deal |
| **Bloomberg** | Mission-critical vertical data | Flat, **unlimited-within-footprint** annual — premium, simple, sticky |
| **Harvey / Sierra / Glean** | Vertical AI agents | Price against the **labour replaced**, not per feature |
| **GenAI dev shops / SIs** | Bespoke AI build | What **not** to become — project/T&M pricing caps you at contractor economics |

**Harvey pricing benchmark (reported, not official — Harvey publishes no price list):**
- Per seat: mid-market firms ~$1,000–$2,000/seat/mo; Am Law 100 volume deals ~$100–$200/seat/mo (≈10× spread).
- Seat minimums ~25–50 (100 for bespoke); 12-month commitments; ACV typically ~$50k–$300k+/yr; renewal uplifts 10–25% if uncapped.
- Lesson: no public price, custom contracts, a minimum floor that qualifies buyers, premium unit price justified by the labour replaced, volume discounts that expand the account.

## The pricing model (current — as set by the client)

Simple **flat monthly**, land-and-expand, **not** project-based:

- **Enterprise Platform subscription: ₹5,00,000 / month** — unlimited users, unlimited standard usage, all divisions, the full & growing dashboard library, forking of existing modules, hosting/security/upgrades/support.
- **New-build add-on: +₹3,00,000 / month** — applies **only while** building a materially new dashboard / agent / pipeline (min ~3 months). Stops when it ships; the module stays and joins the library. Then back to ₹5,00,000.
- **First 3 months = ₹8,00,000 / month** (₹5L platform + ₹3L Land Division build). **From month 4 = ₹5,00,000 / month.**
- Do **not** headline a 12-month/annual total (client preference); show the monthly structure only.
- Terms: 12-month initial term, billed in advance, taxes extra, premium third-party datasets / paid APIs / exceptional infra extra where required. Client owns its data; Munshot owns the platform & reusable components.

### Why this shape works
- **Buy the commodity, build the moat.** Unlimited users/usage/dashboards are cheap to scale → make them unlimited (feels generous). The real cost driver is **coverage** (data sources, geographies) and **new builds** → that's where new money enters.
- **Land cheap, expand big.** The wedge (Land) is priced to land; the value compounds as more divisions come onto the same platform at no new per-user cost.
- **Predictable, flat, annual-feeling** — matches a client who values simplicity and for whom cost is not the concern; avoids per-seat/per-interaction friction.

### Guardrail
"Unlimited" must be scoped to a **defined footprint** (agreed divisions/sources). New geographies or materially new modules are the paid lever (the +₹3L build, or expansion) — otherwise unlimited-flat promises unbounded engineering for a fixed fee.

## Earlier illustrative numbers (superseded, kept for reference)

An earlier per-division framing floated ₹30L setup + ~₹84L/yr run with a ₹1 Cr list and an ₹84L "founding-division" rate, benchmarked to Bloomberg (~$27k/seat/yr) and Palantir (pilot → 7–8-figure expansion). Superseded by the flat ₹5L + ₹3L model above; retained only as context for the value anchor (a 4–5 person land team ≈ ₹1.5–2.5 Cr/yr fully loaded → the platform delivers that output for far less).
