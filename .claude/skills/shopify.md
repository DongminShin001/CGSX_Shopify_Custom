# Skill: Shopify Storefront

## API Rules
- Use **Storefront API only** (never Admin API on frontend)
- GraphQL only — no REST endpoints
- API version: `2025-01`
- All queries go in `app/graphql/` folder

## Authentication
```ts
// Always use Hydrogen's createStorefrontClient
import { createStorefrontClient } from '@shopify/hydrogen';
```

## GraphQL Patterns

### Product Query
```graphql
query ProductQuery($handle: String!) {
  product(handle: $handle) {
    id
    title
    description
    priceRange {
      minVariantPrice {
        amount
        currencyCode
      }
    }
    images(first: 10) {
      nodes {
        url
        altText
        width
        height
      }
    }
    variants(first: 10) {
      nodes {
        id
        title
        availableForSale
        price {
          amount
          currencyCode
        }
      }
    }
  }
}
```

### Collection Query
```graphql
query CollectionQuery($handle: String!, $first: Int!) {
  collection(handle: $handle) {
    id
    title
    description
    products(first: $first) {
      nodes {
        id
        title
        handle
        featuredImage {
          url
          altText
        }
        priceRange {
          minVariantPrice {
            amount
            currencyCode
          }
        }
      }
    }
  }
}
```

## Cart Mutations
```graphql
# Always use these mutations for cart operations
mutation CartCreate($input: CartInput!) {
  cartCreate(input: $input) {
    cart { id checkoutUrl totalQuantity }
    userErrors { field message }
  }
}

mutation CartLinesAdd($cartId: ID!, $lines: [CartLineInput!]!) {
  cartLinesAdd(cartId: $cartId, lines: $lines) {
    cart { id totalQuantity }
    userErrors { field message }
  }
}

mutation CartLinesRemove($cartId: ID!, $lineIds: [ID!]!) {
  cartLinesRemove(cartId: $cartId, lineIds: $lineIds) {
    cart { id totalQuantity }
  }
}
```

## Hydrogen Hooks
```ts
// Cart
import { useCart } from '@shopify/hydrogen';
const { lines, totalQuantity, checkoutUrl, linesAdd, linesRemove } = useCart();

// Money formatting — ALWAYS use this, never format manually
import { Money } from '@shopify/hydrogen';
<Money data={product.priceRange.minVariantPrice} />

// Images — ALWAYS use Shopify CDN
import { Image } from '@shopify/hydrogen';
<Image data={product.featuredImage} sizes="(max-width: 768px) 100vw, 50vw" />
```

## Payments
- Shopify handles ALL payment processing
- Never build custom payment UI
- Redirect to `cart.checkoutUrl` for checkout
- Supported: Shop Pay, Stripe, PayPal, Apple Pay, Google Pay
- Enable in Shopify Admin → Settings → Payments

## Route Patterns (Remix)
```
app/routes/
  _index.tsx              # Homepage / hero gallery
  products.$handle.tsx    # Single product page
  collections.$handle.tsx # Collection/gallery grid
  cart.tsx                # Cart page
  search.tsx              # Search
```

## Error Handling
- Always handle `userErrors` from mutations
- Always handle `null` product (404 case)
- Use Remix `ErrorBoundary` on every route

## Performance
- Use `defer` + `Await` for non-critical data
- Paginate collections (never fetch all at once)
- Always pass `sizes` prop to `<Image>`
- Prefetch product data on hover
