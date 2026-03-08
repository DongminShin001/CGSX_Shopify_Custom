# Project: Custom Shopify Art Gallery Store

## Overview
A fully custom Shopify Hydrogen storefront for selling original art pieces.
Gallery-style UI, premium feel, fully configurable with Shopify backend.

## Stack
- **Framework**: Shopify Hydrogen (Remix-based)
- **Language**: TypeScript (strict mode)
- **Styling**: Tailwind CSS + custom CSS variables
- **API**: Shopify Storefront API (GraphQL only)
- **Payments**: Shopify native checkout (Shop Pay, Stripe, PayPal)
- **Package Manager**: npm

## Project Structure
```
app/
  components/       # Reusable UI components
  routes/           # Remix route files
  lib/              # Utilities, API helpers
  styles/           # Global styles, theme tokens
  graphql/          # All GraphQL queries/mutations
public/             # Static assets
.claude/
  skills/           # Claude skill files
```

## Key Commands
```bash
npm run dev         # Start dev server
npm run build       # Production build
npm run lint        # ESLint
npm run typecheck   # TypeScript check
```

## Code Conventions
- TypeScript strict mode always
- Named exports for components
- Barrel exports (index.ts) per folder
- GraphQL queries in dedicated /graphql folder
- No inline styles — Tailwind only
- Components must be accessible (ARIA labels, keyboard nav)

## Design System
- Gallery-style: clean, minimal, art-forward
- Let the art breathe — generous whitespace
- Dark mode first
- Typography: editorial feel
- Animations: subtle, purposeful

## Environment Variables
```
PUBLIC_STOREFRONT_API_TOKEN=
PUBLIC_STORE_DOMAIN=
PUBLIC_STOREFRONT_API_VERSION=2025-01
```

## Important Notes
- NEVER use Admin API on the frontend — Storefront API only
- Always handle loading and error states
- Cart state managed via Hydrogen's cart hooks
- Images must use Shopify's image CDN (shopifyImageURL helper)
- All prices formatted via Hydrogen's Money component
