# CLAUDE.md — Big Dawgs Automated Quote Flow

## Project

Single-file static site at `index.html`. Renders a mobile-first quote flow for Logan's mobile detailing business (Lake Texoma, OK). Live at https://bigdawgsdetailingllc.github.io/bigdawgs-automated-quote-flow-html/ via GitHub Pages (auto-deploys on push to `main`).

## Integrations live

- **HubSpot Forms API** — portal `245895086`, form GUID `d86a43ad-c65d-48ca-818a-9b025d43347d`. Every quote submit creates a HubSpot contact and triggers the "New Quote Lead Email" workflow → `leads@bigdawgsdetailing.us`.
- **Setmore** — user-managed for booking today. API access request pending (`api@setmore.com` reply awaited). Once approved, `submitLead()` will also POST an appointment.

## Reference docs

- `docs/setmore-services.md` — full catalog of the 50 Setmore services (6 categories) mirroring every recommendation the quote flow can output. Includes the mapping from `A.finalPkgName` → Setmore service name for future auto-booking wire-up. **Read this before touching anything Setmore-related.**

## Quote flow key files/functions in `index.html`

- `recommend()` — three branches by `A.type` (auto/boat/rv). Boat uses the matrix formula `(BOAT_EXT[e].r × t.e + BOAT_INT[i].r × t.i) × ft × ac.mult` with a $350 floor.
- `scrRec()` — renders the recommendation screen, including the upgrade card, ceramic upsell (length-aware on boats: $45–$65/ft), maintenance-plan checkbox, and (boat only) the itemized line-items receipt block.
- `scrOutcome()` — auto shows "YOU'RE ON THE BOOK"; boat/RV show "Thank you for choosing Big Dawgs!" with clickable phone `580-916-5189`.
- `submitLead()` — posts to HubSpot Forms API. Fires for every vehicle type.

## Pricing constants (top of the script block)

- `AUTO_PKG` — auto packages with per-vehicle-class pricing
- `BOAT_EXT`, `BOAT_INT`, `BOAT_TYPES`, `BOAT_ACCESS`, `BOAT_ADDONS`, `BOAT_MINIMUM` — marine matrix
- `RV_BANDS`, `RV_TABLE`, `RV_SYMPTOMS` — RV pricing
- `CERAMIC_START = {auto:399, boat:599, rv:599}` (boat overrides to per-ft at render time)
- `MAINT_PLANS_BY_TYPE` — auto $120/$150/$170, boat/RV $250/$300/$450 per visit
