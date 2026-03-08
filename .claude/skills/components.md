# Skill: Component Patterns

## Core Components to Build

### Must-Have Components
```
components/
  art/
    ArtCard.tsx           # Product card, gallery style
    ArtGrid.tsx           # Masonry/grid layout
    ArtDetail.tsx         # Full product view
    ArtCardSkeleton.tsx   # Loading state

  cart/
    CartDrawer.tsx        # Slide-in cart
    CartLine.tsx          # Single cart item
    CartToggle.tsx        # Cart icon + count badge

  layout/
    Header.tsx            # Floating minimal nav
    Footer.tsx            # Minimal footer
    PageLayout.tsx        # Wrapper with header/footer

  ui/
    Button.tsx            # Primary, secondary, ghost variants
    Badge.tsx             # "Sold Out", "New", etc
    Money.tsx             # Price formatting wrapper
    Image.tsx             # Shopify image wrapper
    Skeleton.tsx          # Loading skeleton base
```

## Button Component
```tsx
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  isLoading?: boolean;
  children: React.ReactNode;
}

const variants = {
  primary: 'bg-white text-black hover:bg-zinc-100',
  secondary: 'border border-zinc-600 text-white hover:border-white',
  ghost: 'text-zinc-400 hover:text-white',
};

const sizes = {
  sm: 'px-4 py-1.5 text-xs tracking-widest',
  md: 'px-8 py-3 text-sm tracking-widest',
  lg: 'px-12 py-4 text-base tracking-widest',
};

export function Button({ variant = 'primary', size = 'md', isLoading, children, ...props }: ButtonProps) {
  return (
    <button
      className={`uppercase transition-colors duration-200 ${variants[variant]} ${sizes[size]} disabled:opacity-50`}
      disabled={isLoading}
      {...props}
    >
      {isLoading ? <LoadingSpinner /> : children}
    </button>
  );
}
```

## Cart Drawer Pattern
```tsx
// Slide-in from right, backdrop blur
<div className={`fixed inset-0 z-50 ${isOpen ? 'pointer-events-auto' : 'pointer-events-none'}`}>
  {/* Backdrop */}
  <div
    className={`absolute inset-0 bg-black transition-opacity duration-300 ${isOpen ? 'opacity-50' : 'opacity-0'}`}
    onClick={onClose}
  />
  {/* Drawer */}
  <div className={`absolute right-0 top-0 h-full w-full max-w-md bg-zinc-950 border-l border-zinc-800 transform transition-transform duration-300 ${isOpen ? 'translate-x-0' : 'translate-x-full'}`}>
    <CartContents />
  </div>
</div>
```

## Sold Out Badge
```tsx
export function SoldOutBadge() {
  return (
    <span className="absolute top-3 left-3 bg-black/80 text-zinc-400 text-xs tracking-widest uppercase px-2 py-1">
      Sold
    </span>
  );
}
```

## Add to Cart Button
```tsx
export function AddToCartButton({ variantId, available }: AddToCartProps) {
  const { linesAdd } = useCart();
  const [adding, setAdding] = useState(false);

  async function handleAdd() {
    setAdding(true);
    await linesAdd([{ merchandiseId: variantId, quantity: 1 }]);
    setAdding(false);
  }

  if (!available) {
    return <Button variant="secondary" disabled>Sold Out</Button>;
  }

  return (
    <Button onClick={handleAdd} isLoading={adding}>
      {adding ? 'Adding...' : 'Add to Collection'}
    </Button>
  );
}
```

## Image with Aspect Ratio
```tsx
// Art pieces often have varying ratios — handle gracefully
export function ArtImage({ data, priority = false }: ArtImageProps) {
  return (
    <div className="relative overflow-hidden bg-zinc-900">
      <Image
        data={data}
        className="w-full h-full object-cover"
        loading={priority ? 'eager' : 'lazy'}
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
      />
    </div>
  );
}
```

## Empty States
```tsx
// Always handle empty collections
export function EmptyCollection() {
  return (
    <div className="flex flex-col items-center justify-center py-32 text-center">
      <p className="font-display text-3xl text-zinc-600 mb-4">No works available</p>
      <p className="text-zinc-500 text-sm mb-8">Check back soon for new pieces.</p>
      <Link to="/" className="text-white underline-offset-4 underline">
        Return home
      </Link>
    </div>
  );
}
```
