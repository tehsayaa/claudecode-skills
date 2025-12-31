---
name: aesthetic
description: Create aesthetically beautiful interfaces and production-ready websites. Use when building actual coded websites (React/Next.js) with AI-generated images, analyzing designs from inspiration sites, generating design mockups for exploration, implementing visual hierarchy and color theory, adding micro-interactions, or creating design documentation. Includes production workflow for building real websites with AI-generated assets (Workflow 0), design exploration workflows for analyzing inspiration and generating mockups (Workflows 1-2), and comprehensive design system guidance covering BEAUTIFUL (aesthetic principles), RIGHT (functionality/accessibility), SATISFYING (micro-interactions), and PEAK (storytelling) stages. Integrates with image-generation, chrome-devtools, ai-multimodal, media-processing, ui-styling, web-frameworks, and frontend-development skills.
---

# Aesthetic

Create aesthetically beautiful interfaces by following proven design principles and systematic workflows.

## When to Use This Skill

Use when:
- **Building production websites** with AI-generated images (hero backgrounds, product images, logos) → Use **Workflow 0**
- **Exploring design concepts** and generating design mockups → Use **Workflow 1 & 2**
- Analyzing designs from inspiration websites (Dribbble, Mobbin, Behance)
- Implementing visual hierarchy, typography, color theory
- Adding micro-interactions and animations
- Creating design documentation and style guides
- Need guidance on accessibility and design systems

### 🎯 Quick Start: Which Workflow Should I Use?

**"I want to build a real website with AI-generated images"**
→ **Use Workflow 0: Build Website with AI-Generated Assets**
- Generates actual Next.js/React code
- Creates AI-generated images for hero, products, features
- Inserts images into proper components
- Production-ready with optimization

**"I want to explore design ideas and see mockups"**
→ **Use Workflows 1 & 2: Design Exploration**
- Generates design mockup images (not code)
- Iterates on visual concepts
- Extracts design tokens from inspiration
- Documents design guidelines

## Core Framework: Four-Stage Approach

### 1. BEAUTIFUL: Understanding Aesthetics
Study existing designs, identify patterns, extract principles. AI lacks aesthetic senseâ€”standards must come from analyzing high-quality examples and aligning with market tastes.

**Reference**: [`references/design-principles.md`](references/design-principles.md) - Visual hierarchy, typography, color theory, white space principles.

### 2. RIGHT: Ensuring Functionality
Beautiful designs lacking usability are worthless. Study design systems, component architecture, accessibility requirements.

**Reference**: [`references/design-principles.md`](references/design-principles.md) - Design systems, component libraries, WCAG accessibility standards.

### 3. SATISFYING: Micro-Interactions
Incorporate subtle animations with appropriate timing (150-300ms), easing curves (ease-out for entry, ease-in for exit), sequential delays.

**Reference**: [`references/micro-interactions.md`](references/micro-interactions.md) - Duration guidelines, easing curves, performance optimization.

### 4. PEAK: Storytelling Through Design
Elevate with narrative elementsâ€”parallax effects, particle systems, thematic consistency. Use restraint: "too much of anything isn't good."

**Reference**: [`references/storytelling-design.md`](references/storytelling-design.md) - Narrative elements, scroll-based storytelling, interactive techniques.

## Workflows

### Workflow 0: Build Website with AI-Generated Assets ⭐ **PRODUCTION**

**Purpose**: Generate actual coded website (React/Next.js) with AI-generated images as assets.

**When to use**: Building a real website (not just design exploration)

**Steps**:
1. **Planning Phase**:
   - Use **chrome-devtools** to capture inspiration screenshots
   - Use **ai-multimodal** to analyze and extract design tokens (colors, typography, spacing)
   - Document findings in design guidelines

2. **Setup Phase**:
   - Use **web-frameworks** skill to initialize Next.js project with App Router
   - Use **ui-styling** skill to set up shadcn/ui with extracted design tokens
   - Configure Tailwind with brand colors and typography

3. **Asset Generation Phase**:
   - Use **image-generation** skill with `--include-css-vars` flag to generate:
     - **Hero section background** (16:9 or 21:9, 4K resolution)
     - **Feature/product images** (1:1 or 16:9, as needed)
     - **Empty state illustrations** (1:1 or 16:9, 2K resolution)
     - **Logo/logomark** (1:1, 4K resolution)
     - **Social media previews** (1:1 or 16:9, 2K resolution)
   - Save all generated images to `public/images/` or `docs/assets/`

4. **Development Phase**:
   - Use **frontend-development** skill to build components
   - Use **ui-styling** skill to implement with shadcn/ui components
   - Insert AI-generated images into appropriate sections:
     ```jsx
     // Hero section with AI-generated background
     <section className="relative h-screen">
       <div className="absolute inset-0">
         <Image
           src="/images/hero-background.png"
           alt="Hero background"
           fill
           priority
         />
       </div>
       <div className="relative z-10">
         <h1>Welcome</h1>
         <p>Hero content goes here</p>
       </div>
     </section>
     ```
   - Ensure proper optimization (Next.js Image component, WebP format)

5. **Quality Assurance**:
   - Test images in actual UI context
   - Verify responsive behavior
   - Check loading performance (use next/image for optimization)
   - Ensure accessibility (alt text, proper sizing)

6. **Iteration** (if needed):
   - Use **Z.ai MCP `ui_diff_check`** to compare generated images with reference designs
   - Use **Z.ai MCP `analyze_image`** to evaluate aesthetic quality in context
   - Regenerate specific assets with **image-generation** skill if score < 7/10
   - Replace images in code

**Example Complete Workflow**:
```bash
# 1. Extract design tokens from inspiration
use ai-multimodal to analyze screenshot and extract design tokens

# 2. Setup project with design tokens
use web-frameworks to initialize Next.js with extracted colors and fonts

# 3. Generate hero background
use image-generation skill with --include-css-vars to generate
"Modern hero background, 16:9, 4K, glassmorphism style"

# 4. Generate product images
use image-generation skill to generate
"Product showcase image, 1:1, 2K, minimalist style"

# 5. Build components with images
use ui-styling to create hero section using generated background

# 6. Build full page
use frontend-development to create complete page structure
```

### Workflow 1: Capture & Analyze Inspiration (Design Exploration)

**Purpose**: Extract design guidelines from inspiration websites.

**Steps**:
1. Browse inspiration sites (Dribbble, Mobbin, Behance, Awwwards)
2. Use **chrome-devtools** skill to capture full-screen screenshots (not full page)
3. Use **Z.ai MCP Vision tools** to analyze screenshots and extract:
   - Use `analyze_image` for general design analysis (layout, colors, style)
   - Use `extract_text_from_screenshot` for typography and text extraction
   - Use `ui_diff_check` to compare multiple designs for consistency
   - Design style (Minimalism, Glassmorphism, Neo-brutalism, etc.)
   - Layout structure & grid systems
   - Typography system & hierarchy
     **IMPORTANT:** Try to predict the font name (Google Fonts) and font size in the given screenshot, don't just use Inter or Poppins.
   - Color palette with hex codes
   - Visual hierarchy techniques
   - Component patterns & styling
   - Micro-interactions
   - Accessibility considerations
   - Overall aesthetic quality rating (1-10)
4. Document findings in project design guidelines using templates

### Workflow 2: Generate & Iterate Design Images

**Purpose**: Create aesthetically pleasing design images through iteration.

**Steps**:
1. Define design prompt with: style, colors, typography, audience, animation specs
2. Use **image-generation** skill with Nano Banana Pro to generate design images
   - Use `--include-css-vars` flag to automatically extract and use project design tokens
   - Use `--match-design-tokens` to align with existing design system
   - Specify appropriate aspect ratio and resolution for intended use
3. Use **Z.ai MCP `analyze_image`** or **Z.ai MCP `ui_diff_check`** to analyze output images and evaluate aesthetic quality
4. If score < 7/10 or fails professional standards:
   - Identify specific weaknesses (color, typography, layout, spacing, hierarchy)
   - Refine prompt with improvements
   - Regenerate with **image-generation** skill or use **media-processing** skill to modify outputs (resize, crop, filters, composition)
5. Repeat until aesthetic standards met (score â‰¥ 7/10)
6. Document final design decisions using templates

## Design Documentation

### Create Design Guidelines
Use [`assets/design-guideline-template.md`](assets/design-guideline-template.md) to document:
- Color patterns & psychology
- Typography system & hierarchy
- Layout principles & spacing
- Component styling standards
- Accessibility considerations
- Design highlights & rationale

Save in project `./docs/design-guideline.md`.

### Create Design Story
Use [`assets/design-story-template.md`](assets/design-story-template.md) to document:
- Narrative elements & themes
- Emotional journey
- User journey & peak moments
- Design decision rationale

Save in project `./docs/design-story.md`.

## Resources & Integration

### Related Skills
- **Z.ai MCP Vision Tools**: `analyze_image`, `extract_text_from_screenshot`, `ui_diff_check` for screenshot analysis and design evaluation
- **image-generation**: Generate high-quality frontend assets using Google Nano Banana Pro. Automatically extracts and uses CSS variables and design tokens from projects. Specialized in UI/UX assets including backgrounds, logos, icons, and illustrations.
- **frontend-development**: Build React/TypeScript applications with modern patterns. Use for creating actual page components and integrating AI-generated images into production code.
- **ui-styling**: Implement designs with shadcn/ui components + Tailwind CSS utility-first styling. Use for building UI components with proper styling and theming.
- **web-frameworks**: Build with Next.js (App Router, Server Components, SSR/SSG). Use for project initialization and configuring the application structure.
- **ai-multimodal**: Analyze documents (PDFs), audio, and video. Note: Use Z.ai MCP Vision tools for image/OCR tasks.
- **chrome-devtools**: Capture full-screen screenshots from inspiration websites, navigate between pages, interact with elements, read console logs & network requests
- **media-processing**: Refine generated images (FFmpeg for video, ImageMagick for images)

### Reference Documentation
**References**: [`references/design-resources.md`](references/design-resources.md) - Inspiration platforms, design systems, AI tools, MCP integrations, development strategies.

## Key Principles

1. Aesthetic standards come from humans, not AIâ€”study quality examples
2. Iterate based on analysisâ€”never settle for first output
3. Balance beauty with functionality and accessibility
4. Document decisions for consistency across development
5. Use progressive disclosure in designâ€”reveal complexity gradually
6. Always evaluate aesthetic quality objectively (score â‰¥ 7/10)
