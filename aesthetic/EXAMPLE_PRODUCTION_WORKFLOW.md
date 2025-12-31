# Example: Building a Production Website with AI-Generated Images

This example demonstrates **Workflow 0** from the aesthetic skill - building a real Next.js website with AI-generated images.

## Scenario

Build a modern SaaS landing page with:
- Hero section with AI-generated background
- Feature section with product images
- Logo
- Social media preview images

## Step-by-Step Execution

### Step 1: Extract Design Tokens from Inspiration

```bash
# Capture inspiration
use chrome-devtools to navigate to dribbble.com
Take screenshot of inspiring landing page

# Analyze and extract design tokens
use ai-multimodal to analyze the screenshot
Extract: colors, typography, spacing, layout patterns
```

**Example Output**:
```json
{
  "colors": {
    "primary": "#3B82F6",
    "secondary": "#8B5CF6",
    "background": "#0F172A",
    "text": "#F8FAFC"
  },
  "typography": {
    "fontFamily": "Inter, sans-serif",
    "headingSize": "2.5rem",
    "bodySize": "1rem"
  },
  "spacing": {
    "borderRadius": "8px",
    "containerPadding": "2rem"
  }
}
```

### Step 2: Initialize Next.js Project

```bash
use web-frameworks skill to initialize Next.js project
- App Router
- TypeScript
- Tailwind CSS
```

### Step 3: Configure Design Tokens

```bash
use ui-styling skill to set up Tailwind with extracted colors
```

**tailwind.config.ts**:
```typescript
export default {
  theme: {
    extend: {
      colors: {
        primary: '#3B82F6',
        secondary: '#8B5CF6',
        background: '#0F172A',
        text: '#F8FAFC',
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
      },
      borderRadius: {
        'lg': '8px',
      },
    },
  },
}
```

### Step 4: Generate AI Images

```bash
# Generate hero background
use image-generation skill with --include-css-vars
Prompt: "Modern SaaS hero background, gradient with primary #3B82F6 and secondary #8B5CF6,
        glassmorphism style, 16:9 aspect ratio, 4K resolution, abstract geometric shapes"

# Save as: public/images/hero-background.png

# Generate feature images
use image-generation skill
Prompt: "SaaS dashboard interface showing analytics, clean minimalist style, 1:1 aspect ratio,
        2K resolution, primary blue accent colors"

# Save as: public/images/feature-dashboard.png

# Generate logo
use image-generation skill
Prompt: "Minimalist logomark for tech SaaS company, abstract geometric shape,
        primary blue #3B82F6, 1:1 aspect ratio, 4K resolution, vector style"

# Save as: public/images/logo.png

# Generate social preview
use image-generation skill
Prompt: "Social media preview for SaaS launch, modern gradient background,
        'Coming Soon' text overlay, 1:1 aspect ratio, 2K resolution"

# Save as: public/images/social-preview.png
```

### Step 5: Build Hero Component with AI Background

**app/page.tsx**:
```typescript
import Image from 'next/image'

export default function HomePage() {
  return (
    <main>
      {/* Hero Section with AI-Generated Background */}
      <section className="relative h-screen flex items-center justify-center overflow-hidden">
        {/* AI-Generated Background Image */}
        <div className="absolute inset-0">
          <Image
            src="/images/hero-background.png"
            alt="Hero background with gradient"
            fill
            priority
            quality={95}
            className="object-cover"
          />
          {/* Dark overlay for text readability */}
          <div className="absolute inset-0 bg-background/50" />
        </div>

        {/* Hero Content */}
        <div className="relative z-10 text-center px-4 max-w-4xl">
          <h1 className="text-5xl font-bold text-text mb-6">
            Build Better Products, Faster
          </h1>
          <p className="text-xl text-text/80 mb-8">
            The all-in-one platform for modern SaaS teams
          </p>
          <button className="bg-primary hover:bg-primary/90 text-white px-8 py-3 rounded-lg font-semibold transition">
            Get Started Free
          </button>
        </div>
      </section>

      {/* Features Section with AI-Generated Images */}
      <section className="py-20 px-4 bg-background">
        <div className="max-w-6xl mx-auto">
          <h2 className="text-3xl font-bold text-text text-center mb-12">
            Powerful Features
          </h2>

          <div className="grid md:grid-cols-3 gap-8">
            {/* Feature 1 */}
            <div className="bg-white/5 backdrop-blur rounded-lg p-6 border border-white/10">
              <div className="aspect-square mb-4 rounded-lg overflow-hidden">
                <Image
                  src="/images/feature-dashboard.png"
                  alt="Analytics Dashboard"
                  width={400}
                  height={400}
                  className="object-cover w-full h-full"
                />
              </div>
              <h3 className="text-xl font-semibold text-text mb-2">
                Advanced Analytics
              </h3>
              <p className="text-text/70">
                Real-time insights and beautiful visualizations
              </p>
            </div>

            {/* More features... */}
          </div>
        </div>
      </section>
    </main>
  )
}
```

### Step 6: Add Logo to Navigation

**components/navbar.tsx**:
```typescript
import Image from 'next/image'
import Link from 'next/link'

export function Navbar() {
  return (
    <nav className="bg-background/80 backdrop-blur-md border-b border-white/10 sticky top-0 z-50">
      <div className="max-w-6xl mx-auto px-4 py-4 flex items-center justify-between">
        {/* AI-Generated Logo */}
        <Link href="/" className="flex items-center gap-2">
          <div className="relative w-10 h-10">
            <Image
              src="/images/logo.png"
              alt="Company Logo"
              fill
              className="object-contain"
            />
          </div>
          <span className="text-text font-bold text-xl">YourBrand</span>
        </Link>

        {/* Navigation links... */}
      </div>
    </nav>
  )
}
```

### Step 7: Add Metadata with Social Preview

**app/layout.tsx**:
```typescript
export const metadata = {
  title: 'YourBrand - Build Better Products',
  description: 'The all-in-one platform for modern SaaS teams',
  openGraph: {
    title: 'YourBrand - Build Better Products',
    description: 'The all-in-one platform for modern SaaS teams',
    images: ['/images/social-preview.png'], // AI-Generated social preview
  },
  twitter: {
    card: 'summary_large_image',
    images: ['/images/social-preview.png'], // AI-Generated social preview
  },
}
```

### Step 8: Quality Assurance

```bash
# Test images in actual UI context
1. Run dev server: npm run dev
2. Navigate to http://localhost:3000
3. Verify hero background loads correctly
4. Check responsive behavior (mobile, tablet, desktop)
5. Test image optimization (Next.js Image component)
6. Verify accessibility (alt text present)

# Evaluate aesthetic quality
use ai-multimodal to screenshot the rendered page
Rate aesthetic quality (target: ≥ 7/10)

# If score < 7/10, iterate:
- Regenerate specific images with refined prompts
- Replace in public/images/
- Test again
```

## Project Structure

```
project-root/
├── app/
│   ├── page.tsx              # Main landing page with AI images
│   ├── layout.tsx            # Root layout with metadata
│   └── globals.css           # Global styles with design tokens
├── components/
│   └── navbar.tsx            # Navigation with AI-generated logo
├── public/
│   └── images/
│       ├── hero-background.png      # AI-generated (16:9, 4K)
│       ├── feature-dashboard.png    # AI-generated (1:1, 2K)
│       ├── logo.png                 # AI-generated (1:1, 4K)
│       └── social-preview.png       # AI-generated (1:1, 2K)
├── tailwind.config.ts       # Design tokens configured
└── next.config.js           # Image optimization enabled
```

## Key Benefits

✅ **Production-Ready Code**: Actual Next.js components, not mockups
✅ **Optimized Images**: Next.js Image component with automatic optimization
✅ **Design Consistency**: All images use extracted design tokens
✅ **Fast Loading**: WebP conversion, lazy loading, proper sizing
✅ **SEO Friendly**: Alt text, semantic HTML, metadata with social previews
✅ **Responsive**: Works on all screen sizes
✅ **Accessible**: Proper ARIA labels, keyboard navigation

## Next Steps

1. **Add More Sections**: About, Pricing, Testimonials (each with AI-generated images)
2. **Create Variants**: Generate multiple hero backgrounds for A/B testing
3. **Optimize Performance**: Convert to WebP format, implement image CDNs
4. **Generate Icons**: Use image-generation for feature icons
5. **Create Illustrations**: Add empty state and onboarding illustrations
