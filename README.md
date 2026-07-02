# Dots Pricing Pages

Static, self-contained HTML pricing pages for **Dots**, with a live cost calculator, region-based pricing, and plan/add-on breakdowns. No build step, no dependencies — each page is a single `.html` file with inline `<style>` and `<script>`.

## Files

`general.html` **Dots Pricing for Teams**  General pricing page — Pilot / Pro / Enterprise plan tiers with a monthly (Pilot) or annual (Pro/Enterprise) calculator.
`index.html` **Dots Pricing for Research Teams**  Research-team variant — single base plan + à la carte add-ons, with its own calculator. |

Both pages share the same visual language (fonts, colors, card layout) but have **independent HTML, CSS, and JS** — they are not generated from a shared template, so any shared change (e.g. a price update, a new region, a color tweak) needs to be made in **both files**.

## Tech stack

- Plain HTML5 + CSS (custom properties / `:root` variables for theming, CSS Grid/Subgrid for layout)
- Vanilla JavaScript (no frameworks, no bundler)
- Google Fonts (`DM Sans`) loaded via CDN
- No external JS libraries — everything (region switching, plan tabs, toggles, calculator math) is hand-rolled

## How it works

### Regions
Pricing is region-based. Both pages define three regions, switched via pill buttons:

- `india` — INR, GST (18%) applied
- `south` — USD, "Asia / Africa" — no GST
- `north` — USD, "North America / Europe" — no GST

Region data (base price, per-user price, add-on prices, currency, GST flag) lives in a `PRICING` object at the bottom of each file's `<script>`. Switching regions re-renders all displayed prices and re-runs the calculator.

### Plans / structure

**`general.html`** (Teams):
- Three plan tiers: **Pilot** (monthly), **Pro** (annual, recommended), **Enterprise** (annual, custom quote).
- A plan-tab calculator lets you pick a plan and estimate cost based on project duration/years, extra users, and add-ons (Collect module + form setups, qualitative/quantitative add-ons, integrations, translation, WhatsApp for Enterprise).
- Enterprise pricing is always shown as "custom" — the calculator swaps to a contact-us panel for that tier.

**`index.html`** (Research Teams):
- A single base plan card + a grid of standalone add-on cards (extra users, Collect module, form setup, qualitative/quantitative bundles).
- One calculator (no plan tabs) that sums platform cost + selected add-ons.
- Includes a "Custom" plan section at the bottom for large-scale/long-term projects (contact form, no self-serve pricing).

### Calculator logic
Each page has its own `calc()` function that:
1. Reads quantities from number inputs / steppers (project duration, extra users, add-on counts, toggles).
2. Multiplies by the active region's unit prices from `PRICING`.
3. Applies GST (18%) if the region requires it.
4. Writes line-item costs and the grand total back into the DOM.

There's also a `resetCalc()` to restore default input values and toggle states.

### Extras
- `general.html` includes a bottom-left **NGO discount popup** that fades in after 15 seconds and links to the contact page, with a dismiss (✕) button.
- Both pages link out to `https://getdots.in/contact` for Enterprise/Custom inquiries.

## Editing pricing

All prices are hardcoded in the `PRICING` object near the bottom of each file's `<script>` tag. To update a price:

1. Open the relevant file (`general.html` or `index.html`).
2. Find the `PRICING` object.
3. Update the value(s) for the relevant region (`india`, `south`, `north`).
4. **Repeat in the other file** if the change should apply to both pages — they don't share state.

## Running locally

These are static files — no server or build step required. Just open them directly in a browser:

```bash
open general.html
# or
open "index.html"
```

(Or drag the file into any browser window.)

## Notes / things to watch

- Pricing logic, plan tiers, and even variable names (`calc`, `resetCalc`, `PRICING`, region keys) differ slightly between the two files since they evolved separately — don't assume a fix in one automatically applies to the other.
- Currency formatting differs by region: INR displays in the `₹X,XX,XXX` / lakh (`L`) format for large numbers, USD displays as `$X,XXX`.
- The India region is GST-inclusive (18%); this is shown as a separate line item in the calculator and totals.
- Consider extracting the shared design tokens (`:root` CSS variables) and pricing data into a shared file if these pages need to stay in sync going forward — right now everything is duplicated per file.

## Suggested next steps for the team

- [ ] Decide if these should be merged into a single templated page (with a query param or toggle for "Teams" vs "Research Teams") to avoid duplicate maintenance.
- [ ] Move `PRICING` data into a shared JSON/config file if both pages need to stay price-synced.
- [ ] Add basic responsive/mobile testing notes if not already covered.
