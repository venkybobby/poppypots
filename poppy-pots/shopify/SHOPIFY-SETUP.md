# Shopify Setup — Poppy Pots

Step-by-step guide to import the demo catalog and theme snippets into a Shopify store.

## Prerequisites

- A Shopify store (Basic plan or higher)
- Admin access to **Products** and **Online Store → Themes**

## 1. Import products

1. Go to **Products → Import**.
2. Upload `poppy-pots-products.csv` from this folder.
3. Review the preview — you should see 3 products with 18 variants each (54 total).
4. Click **Import products**.

### SKU pattern

Every variant follows:

```
PP-{SIZE}-{COLOR}-{PATTERN}
```

| Segment | Values |
|---------|--------|
| SIZE | `S` (Bud), `M` (Bloom), `L` (Grove) |
| COLOR | `TERRACOTTA`, `SAGE`, `CREAM`, `MIDNIGHT`, `BLUSH`, `SAND` |
| PATTERN | `SPECKLE`, `STRIPE`, `ARC` |

Example: `PP-M-SAGE-STRIPE` = Bloom Pot, Sage, Stripe pattern.

## 2. Add theme snippets

Copy the Liquid files into your active theme:

| Source file | Theme destination |
|-------------|-------------------|
| `snippets/pp-free-shipping-bar.liquid` | `snippets/pp-free-shipping-bar.liquid` |
| `sections/pp-marquee.liquid` | `sections/pp-marquee.liquid` |

In the Shopify admin: **Online Store → Themes → Edit code**, then upload or paste each file.

### Free shipping bar

Include the snippet in your theme header or cart drawer:

```liquid
{% render 'pp-free-shipping-bar' %}
```

**Important:** The threshold is `$75` (7500 cents). If you change it, update both:

- `shopify/snippets/pp-free-shipping-bar.liquid` → `pp_free_shipping_threshold`
- `site/index.html` → `FREE_SHIPPING_THRESHOLD`

### Marquee section

1. Go to **Online Store → Customize**.
2. Click **Add section** and select **PP Marquee**.
3. Edit messages and colors to match your brand.

Default colors match the demo design tokens:

- Background: `#8a9a7b` (sage)
- Text: `#ffffff`

## 3. Add product images

The CSV import does not include images. Once product photos are shot:

1. Go to each product in **Products**.
2. Upload images per color variant (recommended: one hero shot per color, pattern visible).
3. Assign images to the matching variants.

Image naming convention (recommended):

```
{handle}-{color}.jpg
```

Examples: `bud-pot-terracotta.jpg`, `bloom-pot-sage.jpg`

## 4. Connect the demo site (optional)

The demo at `site/index.html` is a standalone preview. To go live:

1. Choose a Shopify theme (Dawn or a plant-focused theme works well).
2. Match design tokens from the demo's `:root` CSS variables.
3. Replace the demo checkout with Shopify's native cart and checkout.

## 5. Verify

- [ ] 3 products visible in storefront
- [ ] 18 variants per product with correct SKUs
- [ ] Free shipping bar shows at $75 threshold
- [ ] Marquee section scrolls on homepage
- [ ] Product images assigned to variants

## Known gaps

- No product photos yet (placeholder pot shapes in demo)
- Demo checkout is not connected to Shopify Payments
- Inventory counts are set to 25 as placeholders — update before launch
