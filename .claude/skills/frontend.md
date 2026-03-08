# Skill: Frontend (React + TypeScript + Tailwind)

## Stack
- React 18 (Remix via Hydrogen)
- TypeScript strict mode
- Tailwind CSS (utility classes only)
- No CSS-in-JS, no styled-components

## Component Rules
```ts
// Named exports always
export function ProductCard({ product }: ProductCardProps) {}

// Props interface above component
interface ProductCardProps {
  product: ProductFragment;
  priority?: boolean;
}

// Default props via destructuring
export function Button({ variant = 'primary', size = 'md', ...props }) {}
```

## File Structure per Component
```
components/
  ProductCard/
    index.ts          # barrel export
    ProductCard.tsx   # component
    ProductCard.test.tsx
```

## TypeScript Rules
- Strict mode always
- No `any` — use `unknown` and narrow
- Type all component props explicitly
- Use Shopify's generated types from GraphQL

```ts
// Good
import type { ProductFragment } from '~/graphql/generated';

// Never do this
const product: any = data.product;
```

## Tailwind Conventions
```tsx
// Responsive: mobile first
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

// Dark mode variants
<div className="bg-white dark:bg-zinc-900 text-zinc-900 dark:text-white">

// Group hover for card interactions
<div className="group">
  <img className="group-hover:scale-105 transition-transform duration-500" />
</div>

// Never use arbitrary values unless necessary
// Bad:  w-[347px]
// Good: w-full max-w-md
```

## Animation Patterns
```tsx
// Subtle fade-in on mount (use sparingly)
<div className="animate-fade-in">

// Image zoom on hover (gallery standard)
<div className="overflow-hidden">
  <img className="transition-transform duration-700 ease-out group-hover:scale-110" />
</div>

// Staggered list reveal
{items.map((item, i) => (
  <div
    key={item.id}
    style={{ animationDelay: `${i * 100}ms` }}
    className="animate-fade-in-up"
  />
))}
```

## Accessibility
- Every `<img>` must have meaningful `alt` text
- Interactive elements must be keyboard navigable
- Use semantic HTML (nav, main, section, article)
- ARIA labels on icon-only buttons
- Focus rings never removed (only restyled)

```tsx
// Good
<button aria-label="Add to cart" onClick={handleAdd}>
  <CartIcon aria-hidden="true" />
</button>
```

## Loading States
```tsx
// Always handle loading
{isLoading ? <ProductCardSkeleton /> : <ProductCard product={product} />}

// Skeleton pattern
export function ProductCardSkeleton() {
  return (
    <div className="animate-pulse">
      <div className="aspect-square bg-zinc-200 dark:bg-zinc-800 rounded-lg" />
      <div className="h-4 bg-zinc-200 dark:bg-zinc-800 rounded mt-3 w-2/3" />
    </div>
  );
}
```

## Forms
- Never use HTML `<form>` with default submit
- Use Remix `<Form>` for server actions
- Validate both client and server side
- Always show field-level errors
