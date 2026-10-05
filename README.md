<h1 align="center">Hydrogen D2C Starter</h1>

<p align="center">
  <b>Production-grade Shopify Hydrogen + React Router 7 starter for D2C brands.</b><br/>
  CRO-tested PDP / collection / home layouts · GA4 dataLayer wiring · Pack-style component library · ready to fork.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-black.svg" alt="MIT License"></a>
  <a href="https://github.com/Nuraveda-Labs/hydrogen-d2c-starter"><img src="https://img.shields.io/badge/github-Nuraveda--Labs-181717.svg?logo=github&logoColor=white" alt="GitHub"></a>
  <a href="https://hydrogen.nuraveda.com"><img src="https://img.shields.io/badge/demo-hydrogen.nuraveda.com-00ff88.svg" alt="Live demo"></a>
  <img src="https://img.shields.io/badge/Shopify-Hydrogen-7AB55C?logo=shopify&logoColor=white" alt="Shopify Hydrogen">
  <img src="https://img.shields.io/badge/React_Router-7-CA4245?logo=reactrouter&logoColor=white" alt="React Router 7">
  <img src="https://img.shields.io/badge/Vite-6-646CFF?logo=vite&logoColor=white" alt="Vite">
  <img src="https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white" alt="TypeScript">
</p>

> Built and battle-tested by [Tejas Karan Agrawal](https://github.com/floating-astronaut) on a live Shopify storefront. Brand copy stripped, components left intact — fork it and ship a D2C site without rebuilding the boring parts.

---

## What you get

Hydrogen's `@shopify/hydrogen` quickstart is bare. This starter ships the layer above it — the design-system bones a serious D2C brand actually needs:

### PDP (Product Detail Page)
- Split editorial hero — sticky gallery on desktop, stacked on mobile
- Variant picker + quantity + subscribe card
- Trust band (badges, mini reviews, money-back signal)
- "How it works" timeline + "Who is it for" + "Why this works" sections
- Sticky add-to-cart that appears below the fold
- Reviews block + FAQ accordion + payment trust + shipping/returns
- Closing CTA + brand strip
- Per-product custom landing hook point (drop in a branded landing for specific handles)

### Collection page
- Editorial hero with subhead + breadcrumb
- Product grid with badges (`bestseller`, `new`, sale price highlight)
- Title scrubbing utility for Shopify titles that carry awkward `|` separators

### Home page
- Hero band with eyebrow, big headline, supporting copy, primary CTA
- Mission band (3-up value pillars)
- Real-customer video strip
- "Shop the shelf" carousel
- Proof strip (press logos / social proof)
- How-it-works rail
- Multi-row marquee
- Editorial breaks between sections

### Cart
- Slide-over cart drawer with line items, qty, totals, checkout CTA
- Empty-cart state

### Analytics
- **GA4 dataLayer subscriber** (`app/lib/analytics/`) — a single self-contained file that subscribes to Hydrogen's first-party Analytics events and emits clean GA4 events (`view_item`, `add_to_cart`, `begin_checkout`, etc.). Pack-style pattern, but vanilla — no third-party SDK.

### Customer accounts
- Login / logout / authorize routes wired against Shopify Customer Account API
- Orders list + order detail + addresses + profile pages

### Build & deploy
- Vite 6 + Hydrogen build → Oxygen-ready output in `dist/`
- Reverse-proxiable behind Nginx + Cloudflare Tunnel (production-tested)
- Strict CSP in `app/entry.server.tsx`
- ESLint + Prettier + GraphQL codegen + TypeScript strict

---

## Quick start

**Requirements:** Node 18+, pnpm

```bash
git clone https://github.com/Nuraveda-Labs/hydrogen-d2c-starter.git
cd hydrogen-d2c-starter
pnpm install
cp .env.example .env       # fill with your Shopify Storefront API keys
pnpm dev                   # http://localhost:3000
```

### Required env vars (`.env`)
```
SESSION_SECRET=                              # any random string
PUBLIC_STORE_DOMAIN=your-store.myshopify.com
PUBLIC_STOREFRONT_API_TOKEN=                 # Shopify Storefront API token
PUBLIC_CHECKOUT_DOMAIN=your-store.myshopify.com
PUBLIC_CUSTOMER_ACCOUNT_API_CLIENT_ID=       # for customer-accounts routes
PUBLIC_CUSTOMER_ACCOUNT_API_URL=
```

Pull these automatically from your Shopify store:
```bash
pnpm shopify hydrogen link
pnpm shopify hydrogen env pull
```

---

## Layout

```
app/
  routes/                          # React Router 7 file routes
    _index.tsx                       Home
    products.$handle.tsx             Generic PDP
    collections.$handle.tsx          Collection
    collections.all.tsx              All products
    cart.tsx                         Cart drawer routes
    account.*.tsx                    Customer account
  components/
    pdp/                             Product page sections
      RichTrustBand, HowItWorksTimeline, WhoIsItFor,
      WhyThisWorks, Reviews, FaqAccordion, ShippingReturns,
      StickyAtc, PaymentTrust, BrandStrip, ClosingCta
    home/
      MissionBand, ShopTheShelf, ProofStrip, RealDogsVideo,
      ProductRange, MultiRowMarquee
    collection/
      CollectionHero
    motion/
      ScrollReveal                   IntersectionObserver-backed reveal
  graphql/                         Storefront API queries
  lib/                             Helpers (analytics, redirects, etc.)
  styles/
    app.css                          Global Tailwind layer + D2C-prefixed utilities
docs/                              Architecture notes
scripts/                           Dev utilities
```

---

## Make it yours

This starter is intentionally generic. Copy that survived the brand scrub is marked with placeholder text like *"Your tagline here"* or *"Demo product description"*. Walk through these to rebrand:

1. `package.json` — `name`, `description`, `author`, `homepage`
2. `app/routes/_index.tsx` — home page hero copy, mission band pillars, section headers
3. `app/routes/products.$handle.tsx` — `meta` title/description, `OUTCOMES` array
4. `app/components/Footer.tsx` — tagline + elevator pitch
5. `app/components/Header.tsx` — logo / brand mark
6. `app/styles/app.css` — color tokens (look for the `--ink`, `--paper`, `--line` custom properties)
7. `public/brand/` — replace logo and brand assets
8. `app/lib/analytics/` — wire your GA4 measurement ID

### Custom landing pages
The generic PDP serves every product. To render a dedicated landing for one product handle, hook in at `products.$handle.tsx`:

```tsx
import {HeroLanding} from '~/components/pdp/hero/HeroLanding';
const HERO_HANDLE = 'your-flagship-product-handle';

// inside Product() before the generic return:
if (product.handle === HERO_HANDLE) {
  return <><HeroLanding {...} /><Analytics.ProductView .../></>;
}
```

---

## Deploy

This starter is **Oxygen-ready** (`pnpm build` produces a Worker bundle). It also runs as a long-lived Node process behind Nginx for traditional VPS deploys — that's how `hydrogen.nuraveda.com` is hosted.

**Oxygen (Shopify-hosted):**
```bash
pnpm shopify hydrogen deploy
```

**VPS / self-hosted:**
```bash
pnpm build && pnpm preview --host 127.0.0.1 --port 3001
# proxy from Nginx, terminate TLS at Cloudflare or local certbot
```

---

## Why this exists

D2C brands keep rebuilding the same Hydrogen scaffolding — trust bands, FAQ accordions, sticky ATC, mission strips, customer-account flows. None of it is novel; all of it is fiddly. This starter packages the parts that survived being deployed on a real production storefront, scrubbed of brand-specific copy, so the next team doesn't pay the same setup tax.

This was originally built for an Ayurvedic pet-wellness brand. The vertical is gone from the codebase; the layouts and patterns that worked are still here. Fork, restyle, ship.

---

## License

[MIT](LICENSE) — use it commercially, fork it, modify it. Attribution appreciated but not required.

---

<p align="center">
  <sub>
    Maintained by <a href="https://github.com/floating-astronaut">Tejas Karan Agrawal</a> · 
    Independent AI lab shipping <a href="https://glitchexecutor.com">Glitch Executor</a> and <a href="https://meshpilot.app">Mesh Pilot</a>.
  </sub>
</p>
