---
'@tanstack/cli': minor
'@tanstack/create': minor
---

feat(cli, create): add Shopify storefront add-on + storefront template

Headless Shopify support for TanStack Start apps, scaffold-ready and
runtime-portable. The pitch: prove that TanStack Start is a first-class
target for Shopify, not just Next.js Commerce or Hydrogen.

**`shopify` add-on** — additive. `tanstack add shopify` mounts `/shop/*`
routes alongside an existing app without touching the home page. Includes:

- Storefront API client (server-only fetch via `createServerFn`, public
  token by default + optional private token for higher rate limits and
  buyer-IP forwarding).
- Hand-written GraphQL queries with hydrogen-react types (type-only;
  zero runtime weight).
- httpOnly cookie cart (`tanstack_cart_id`) + React Query single-key
  cache + optimistic updates with module-level mutation counter to
  batch invalidations during rapid clicks.
- Hydrogen-demo parity routes: shop landing, product detail (with
  variants + availability), collections, cart, search, Shopify CMS
  pages, policies.
- Hydrogen-stock UI components (ProductCard, VariantSelector,
  AddToCartButton, CartLineItem, CartSummary, ShopImage with CDN
  transforms, Money via Intl) themed with six CSS custom properties
  for easy reskinning.
- Header cart-count badge via the `header-user` integration slot.
- Shopify-hosted checkout (redirect to `cart.checkoutUrl`).
- **Optional Customer Account API** behind a `customerAccount` select
  option. Hand-rolled OAuth 2.1 PKCE with `.well-known` discovery
  cached in module memory (no usable npm client exists yet),
  HMAC-signed httpOnly session cookies (HS256), lazy token refresh,
  account dashboard / orders / order detail / addresses routes — all
  EJS-guarded so the files only emit when enabled.

**`shopify-storefront` template** — storefront-first.
`tanstack create my-shop --template shopify-storefront` cascades the
`shopify` add-on (which cascades `tanstack-query`) and replaces the
home route with a polished landing (hero + featured collections + best
sellers grid).

**Zero-config first run.** Defaults to Shopify's public Hydrogen demo
store (`hydrogen-preview.myshopify.com`) so the storefront renders
real products immediately. Override the four env vars in `.env.local`
(or your deploy target's dashboard) to point at your store. Demo
defaults are baked into source as fallbacks, so the experience doesn't
break when a runtime doesn't load `.env` files into `process.env`.

**Portable.** Cookie ops via `@tanstack/react-start/server`; crypto via
Web Crypto (`crypto.subtle`); generic `CDN-Cache-Control` for browse
(`s-maxage=300, stale-while-revalidate=600`) and `private, no-store`
for cart. Works on Node, Cloudflare Workers, Shopify Oxygen (just
Workers), Vercel, Netlify, Bun, Deno.

**Header layout fix.** While the cart-count badge is the new
right-aligned action, the base scaffold's `Header` was placing the
social icons left-of-center on `sm+`. Reordered the JSX so navigation
sits between the logo and the right-side actions in DOM order, with
one mobile-only `order-3` to keep `flex-wrap` putting nav on its own
row. Result: logo → nav → (auto-spaced) → cart/social/theme on every
breakpoint, and a more sensible reading order for screen readers.
