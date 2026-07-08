# AGENTS.md — Poppy Pots dev conventions

Rules for AI-assisted development on this project. Cursor also loads these via `.cursorrules`.

## General rules

1. **Changed-code-only** — Edit only files required by the task. No drive-by refactors.
2. **Stay in scope** — Do not add features, dependencies, or files the user did not ask for.
3. **Ask, don't assume** — If a requirement is ambiguous, ask before building.
4. **Simplest solution first** — Prefer the smallest correct diff. No premature abstraction.
5. **FILES CHANGED summary** — End every response with a `FILES CHANGED` block listing what was touched and what was not.

## Project invariants

### Single-file demo

All demo site logic lives in `site/index.html`. Do not split into separate CSS/JS files unless explicitly asked.

### Design tokens

Route all colors through CSS custom properties (`var(--pp-*)`). Never hardcode hex in styles when a token exists.

### Data objects

Catalog changes go through `PRODUCTS`, `COLORS`, and `ART` in the `<script>` block. Do not duplicate product data elsewhere in the demo.

### SKU pattern

```
PP-{SIZE}-{COLOR}-{PATTERN}
```

When adding products or variants, follow this pattern in both the demo (`buildSku()`) and the Shopify CSV.

### Free shipping threshold

`$75` — keep synced in:

- `site/index.html` → `FREE_SHIPPING_THRESHOLD`
- `shopify/snippets/pp-free-shipping-bar.liquid` → `pp_free_shipping_threshold`

If you change one, change both.

### Demo checkout

The checkout is a demo modal only. Never wire real payment processing, Stripe, or Shopify Buy Button into `site/index.html`.

## Adding a product (recipe)

### 1. Demo site

```javascript
// In site/index.html <script> block:
PRODUCTS.push({
  id: 'new-id',
  name: 'New Pot',
  size: 'M',
  sizeLabel: 'Medium · 6"',
  price: 40,
  height: 160,
});
```

If new color or pattern needed, add to `COLORS` or `ART` first.

### 2. Shopify CSV

Add rows to `shopify/poppy-pots-products.csv`:

- One product row with Title, Body, Vendor, etc.
- One variant row per Color × Pattern combo
- SKU: `PP-M-NEWWCOLOR-SPECKLE` etc.

### 3. Verify

- Open `site/index.html` in browser — new product appears in grid
- Check SKU in cart matches CSV
- Re-import CSV or add variants in Shopify admin

## File map

| File | Touch when… |
|------|-------------|
| `site/index.html` | Demo UI, product data, cart, checkout |
| `shopify/poppy-pots-products.csv` | Product catalog for Shopify import |
| `shopify/snippets/pp-free-shipping-bar.liquid` | Shipping threshold or bar styling |
| `shopify/sections/pp-marquee.liquid` | Scrolling banner messages or styling |
| `shopify/SHOPIFY-SETUP.md` | Setup instructions change |
| `README.md` | Architecture or token docs change |
| `AGENTS.md` / `.cursorrules` | Convention changes |

## Response format

End task responses with:

```
FILES CHANGED: file1 (what changed), file2 (what changed)
NOT TOUCHED: files left alone
CONCERNS: blockers or follow-ups (or "none")
```
