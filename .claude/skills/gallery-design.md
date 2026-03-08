# Skill: Art Gallery Design System

## Design Philosophy
- Art is the hero — UI gets out of the way
- Generous whitespace, editorial typography
- Dark mode first (makes art pop)
- Minimal chrome, maximum impact
- Every interaction should feel intentional

## Color Tokens
```css
:root {
  /* Background */
  --color-bg-primary: #0a0a0a;
  --color-bg-secondary: #111111;
  --color-bg-surface: #1a1a1a;

  /* Text */
  --color-text-primary: #f5f5f5;
  --color-text-secondary: #a3a3a3;
  --color-text-muted: #525252;

  /* Accent */
  --color-accent: #e8d5b0;        /* warm gold for art context */
  --color-accent-hover: #f0e0c0;

  /* Borders */
  --color-border: #262626;
  --color-border-subtle: #1f1f1f;
}
```

## Typography
```css
/* Display: for titles, hero text */
font-family: 'Cormorant Garamond', serif;  /* elegant, editorial */

/* Body: for descriptions, UI */
font-family: 'DM Sans', sans-serif;        /* clean, readable */

/* Mono: for prices, metadata */
font-family: 'DM Mono', monospace;
```

Load via Google Fonts:
```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300;1,400&family=DM+Sans:wght@300;400;500&family=DM+Mono:wght@300;400&display=swap" rel="stylesheet">
```

## Gallery Grid Layout
```tsx
// Masonry-style art grid
<div className="columns-1 md:columns-2 lg:columns-3 gap-4 space-y-4">
  {products.map(product => (
    <div key={product.id} className="break-inside-avoid">
      <ProductCard product={product} />
    </div>
  ))}
</div>

// Or CSS Grid with variable heights
<div className="grid grid-cols-2 lg:grid-cols-3 auto-rows-auto gap-3">
```

## Product Card (Art Style)
```tsx
export function ArtCard({ product }: ArtCardProps) {
  return (
    <article className="group relative cursor-pointer">
      {/* Image container */}
      <div className="overflow-hidden bg-zinc-900">
        <Image
          data={product.featuredImage}
          className="w-full object-cover transition-transform duration-700 group-hover:scale-105"
          sizes="(max-width: 768px) 100vw, 33vw"
        />
      </div>

      {/* Hover overlay */}
      <div className="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-colors duration-300 flex items-end">
        <div className="p-4 translate-y-2 group-hover:translate-y-0 opacity-0 group-hover:opacity-100 transition-all duration-300">
          <button className="bg-white text-black px-6 py-2 text-sm tracking-widest uppercase">
            View Piece
          </button>
        </div>
      </div>

      {/* Info below image */}
      <div className="mt-3 flex justify-between items-start">
        <div>
          <h3 className="font-display text-lg text-zinc-100">{product.title}</h3>
          <p className="text-zinc-500 text-sm mt-0.5">Original artwork</p>
        </div>
        <Money
          data={product.priceRange.minVariantPrice}
          className="font-mono text-sm text-zinc-300"
        />
      </div>
    </article>
  );
}
```

## Hero Section
```tsx
// Full-bleed hero with featured art piece
<section className="relative h-screen overflow-hidden">
  <Image
    data={heroImage}
    className="absolute inset-0 w-full h-full object-cover"
    sizes="100vw"
  />
  <div className="absolute inset-0 bg-gradient-to-b from-black/20 via-transparent to-black/60" />
  <div className="absolute bottom-16 left-8 md:left-16 max-w-lg">
    <p className="text-zinc-400 text-sm tracking-[0.3em] uppercase mb-4">
      New Collection
    </p>
    <h1 className="font-display text-5xl md:text-7xl text-white leading-none mb-6">
      Original Art
    </h1>
    <Link to="/collections/all" className="inline-flex items-center gap-3 text-white border-b border-white/40 pb-1 hover:border-white transition-colors">
      Explore Collection
      <ArrowRight size={16} />
    </Link>
  </div>
</section>
```

## Navigation
```tsx
// Minimal floating nav
<nav className="fixed top-0 left-0 right-0 z-50 flex justify-between items-center px-8 py-6">
  <Link to="/" className="font-display text-xl tracking-wide">
    Studio Name
  </Link>
  <div className="flex gap-8 text-sm text-zinc-400">
    <Link to="/collections/all" className="hover:text-white transition-colors">Work</Link>
    <Link to="/about" className="hover:text-white transition-colors">About</Link>
    <CartToggle />
  </div>
</nav>
```

## Product Detail Page
- Large image left (or fullscreen scroll)
- Minimal info right: title, medium, size, price
- Single CTA: "Add to Collection"
- Related works below as horizontal scroll

## Spacing Scale (Tailwind)
```
Section padding:   py-24 md:py-32
Content max-width: max-w-7xl mx-auto px-6 md:px-12
Grid gap:          gap-3 md:gap-4
Card spacing:      space-y-3
```

## Micro-interactions
- Image zoom on hover: `group-hover:scale-105 duration-700`
- Link underline slide: border-b with width transition
- Cart icon pulse on add
- Page transitions: fade between routes
