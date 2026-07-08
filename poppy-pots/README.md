# Poppy Pots

Handcrafted ceramic planters — demo storefront and Shopify import package.

## Quick start

Open the demo site directly in a browser:

```
site/index.html
```

No build step, no server required. Double-click or drag into a browser tab.

## Project structure

```
poppy-pots/
├── README.md              ← you are here
├── AGENTS.md              ← dev conventions for AI-assisted work
├── .cursorrules           ← same rules, auto-loaded by Cursor
├── site/
│   └── index.html         ← single-file demo storefront
└── shopify/
    ├── poppy-pots-products.csv
    ├── sections/pp-marquee.liquid
    ├── snippets/pp-free-shipping-bar.liquid
    └── SHOPIFY-SETUP.md
```

## Architecture

The demo site is a **single-file invariant** — all HTML, CSS, and JavaScript live in `site/index.html`. No bundler, no framework, no external dependencies beyond Google Fonts.

### Data objects

Three JavaScript constants drive the entire catalog:

| Object | Purpose | Location in `index.html` |
|--------|---------|--------------------------|
| `PRODUCTS` | Catalog entries (name, size, price) | `<script>` block |
| `COLORS` | Glaze palette (name + hex) | `<script>` block |
| `ART` | Surface patterns (name + CSS gradient) | `<script>` block |

#### PRODUCTS

```javascript
const PRODUCTS = [
  { id: 'bud',   name: 'Bud Pot',   size: 'S', sizeLabel: 'Small · 4"',  price: 24 },
  { id: 'bloom', name: 'Bloom Pot', size: 'M', sizeLabel: 'Medium · 6"', price: 36 },
  { id: 'grove', name: 'Grove Pot', size: 'L', sizeLabel: 'Large · 8"',  price: 52 },
];
```

#### COLORS

```javascript
const COLORS = {
  terracotta: { name: 'Terracotta', hex: '#c4714a' },
  sage:       { name: 'Sage',       hex: '#8a9a7b' },
  // ...
};
```

#### ART

```javascript
const ART = {
  speckle: { name: 'Speckle', css: 'repeating-radial-gradient(...)' },
  stripe:  { name: 'Stripe',  css: 'repeating-linear-gradient(...)' },
  arc:     { name: 'Arc',     css: 'radial-gradient(...)' },
};
```

### SKU pattern

```
PP-{SIZE}-{COLOR}-{PATTERN}
```

- SIZE: `S`, `M`, `L`
- COLOR: uppercase color key (e.g. `TERRACOTTA`)
- PATTERN: uppercase art key (e.g. `SPECKLE`)

Built in JS via `buildSku(product, colorKey, artKey)`.

### Design tokens

All colors route through CSS custom properties in `:root`. Never hardcode hex values in component styles — use `var(--pp-*)`.

| Token | Value | Usage |
|-------|-------|-------|
| `--pp-cream` | `#f7f3ee` | Page background |
| `--pp-terracotta` | `#c4714a` | Primary accent, CTA buttons |
| `--pp-terracotta-dark` | `#a85a38` | Hover states |
| `--pp-sage` | `#8a9a7b` | Secondary accent, marquee |
| `--pp-sage-dark` | `#6d7d5f` | Qualified shipping text |
| `--pp-midnight` | `#2c3e50` | Header bar, checkout |
| `--pp-blush` | `#e8c4b8` | Highlights, demo badge |
| `--pp-sand` | `#d4c4a8` | Glaze color option |
| `--pp-charcoal` | `#3a3a3a` | Body text |
| `--pp-text-muted` | `#6b6b6b` | Secondary text |
| `--pp-border` | `#e0d8cf` | Card borders |
| `--pp-font-display` | Cormorant Garamond | Headings |
| `--pp-font-body` | DM Sans | Body copy |
| `--pp-radius` | `12px` | Cards, panels |
| `--pp-radius-sm` | `8px` | Buttons, swatches |

### Free shipping threshold

**$75** — must stay synced in two places:

1. `site/index.html` → `FREE_SHIPPING_THRESHOLD = 75`
2. `shopify/snippets/pp-free-shipping-bar.liquid` → `pp_free_shipping_threshold = 7500` (cents)

### Demo checkout

The checkout button opens a modal that says "Demo Only — no payment processed." This is intentional. Do not wire real payment processing into the demo site.

## Adding a product

### Demo site (`site/index.html`)

1. Add an entry to `PRODUCTS` with a unique `id`, `size` letter, and `price`.
2. If adding a new color, add it to `COLORS` with `name` and `hex`.
3. If adding a new pattern, add it to `ART` with `name` and `css` gradient.
4. The product grid and detail panel render automatically from the data objects.
5. SKU is generated via `buildSku()` — no manual SKU needed in JS.

### Shopify (`shopify/poppy-pots-products.csv`)

1. Add a new product block with a unique `Handle`.
2. Create variant rows for every Color × Pattern combination.
3. Follow the SKU pattern: `PP-{SIZE}-{COLOR}-{PATTERN}`.
4. Re-import via **Products → Import** (or add manually in admin).
5. See `shopify/SHOPIFY-SETUP.md` for full import steps.

## Known gaps

| Gap | Status | Suggested next step |
|-----|--------|---------------------|
| Product photos | Placeholder pot shapes (CSS) | Wire real photos into variant structure once shot |
| Shopify theme | Snippets only, no full theme | Install snippets + import CSV, pick a base theme |
| Demo checkout | Modal only, no payment | Connect Shopify native checkout when going live |
| Inventory | Placeholder count of 25 | Set real stock levels before launch |

## Shopify

See `shopify/SHOPIFY-SETUP.md` for import instructions, snippet installation, and verification checklist.
