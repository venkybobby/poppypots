# Poppy Pots

Handcrafted ceramic planters — demo storefront and Shopify import package.

## Getting started

All project files live in [`poppy-pots/`](poppy-pots/):

```
poppy-pots/
├── README.md          ← architecture, design tokens, known gaps
├── AGENTS.md          ← dev conventions for AI-assisted work
├── .cursorrules       ← same rules, auto-loaded by Cursor
├── site/
│   └── index.html     ← working demo (open directly in a browser)
└── shopify/
    ├── poppy-pots-products.csv
    ├── sections/pp-marquee.liquid
    ├── snippets/pp-free-shipping-bar.liquid
    └── SHOPIFY-SETUP.md
```

**Open the demo:**

- **Live (after Pages is enabled):** https://venkybobby.github.io/poppypots/
- **Local:** open `poppy-pots/site/index.html` in your browser (File → Open, or double-click)

> **First-time setup:** In GitHub go to **Settings → Pages → Build and deployment → Source** and choose **GitHub Actions**. The deploy workflow runs automatically on push to `main`.

**Full docs:** [`poppy-pots/README.md`](poppy-pots/README.md)
