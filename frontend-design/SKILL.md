---
name: frontend-design
description: Create distinctive, production-grade frontend interfaces with high design quality and automatic image sourcing. Use this skill when building web components, pages, or applications. Generates creative, polished code with real images from Unsplash/Pexels via web search or AI-generated images via image-generation skill. Avoids generic AI aesthetics.
license: Complete terms in LICENSE.txt
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
---

This skill guides creation of distinctive, production-grade frontend interfaces with automatic image acquisition. Use web search tools to find real images from Unsplash, Pexels, and other stock photo sites, or leverage image-generation skill for custom AI-generated assets. Implement real working code with exceptional attention to aesthetic details.

The user provides frontend requirements: a component, page, application, or interface to build. They may include context about the purpose, audience, or technical constraints.

## Design Thinking

Before coding, understand the context and commit to a BOLD aesthetic direction:
- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Pick an extreme: brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian, etc. There are so many flavors to choose from. Use these for inspiration but design one that is true to the aesthetic direction.
- **Constraints**: Technical requirements (framework, performance, accessibility).
- **Differentiation**: What makes this UNFORGETTABLE? What's the one thing someone will remember?

**CRITICAL**: Choose a clear conceptual direction and execute it with precision. Bold maximalism and refined minimalism both work - the key is intentionality, not intensity.

Then implement working code (HTML/CSS/JS, React, Vue, etc.) that is:
- Production-grade and functional
- Visually striking and memorable
- Cohesive with a clear aesthetic point-of-view
- Meticulously refined in every detail

## Frontend Aesthetics Guidelines

Focus on:
- **Typography**: Choose fonts that are beautiful, unique, and interesting. Avoid generic fonts like Arial and Inter; opt instead for distinctive choices that elevate the frontend's aesthetics; unexpected, characterful font choices. Pair a distinctive display font with a refined body font.
- **Color & Theme**: Commit to a cohesive aesthetic. Use CSS variables for consistency. Dominant colors with sharp accents outperform timid, evenly-distributed palettes.
- **Motion**: Use animations for effects and micro-interactions. Prioritize CSS-only solutions for HTML. Use Motion library for React when available (Use `anime.js` for animations: `./references/animejs.md`). Focus on high-impact moments: one well-orchestrated page load with staggered reveals (animation-delay) creates more delight than scattered micro-interactions. Use scroll-triggering and hover states that surprise.
- **Spatial Composition**: Unexpected layouts. Asymmetry. Overlap. Diagonal flow. Grid-breaking elements. Generous negative space OR controlled density.
- **Backgrounds & Visual Details**: Create atmosphere and depth rather than defaulting to solid colors. Add contextual effects and textures that match the overall aesthetic. Apply creative forms like gradient meshes, noise textures, geometric patterns, layered transparencies, dramatic shadows, decorative borders, custom cursors, and grain overlays.

NEVER use generic AI-generated aesthetics like overused font families (Inter, Roboto, Arial, system fonts), cliched color schemes (particularly purple gradients on white backgrounds), predictable layouts and component patterns, and cookie-cutter design that lacks context-specific character.

Interpret creatively and make unexpected choices that feel genuinely designed for the context. No design should be the same. Vary between light and dark themes, different fonts, different aesthetics. NEVER converge on common choices (Space Grotesk, for example) across generations.

**IMPORTANT**: Match implementation complexity to the aesthetic vision. Maximalist designs need elaborate code with extensive animations and effects. Minimalist or refined designs need restraint, precision, and careful attention to spacing, typography, and subtle details. Elegance comes from executing the vision well.

## Image & Visual Asset Requirements

### Mandatory Image Sourcing

Every design MUST include real, high-quality images. NEVER use placeholders or generic lorem ipsum images.

**Image Acquisition Strategy (in priority order)**:

1. **Search Unsplash/Pexels via web search tools**:
   - Query: `"site:unsplash.com [topic] [style]"` or `"site:pexels.com [topic] [style]"`
   - Unsplash URL format: `https://images.unsplash.com/photo-[id]?w=[width]&h=[height]&fit=crop`
   - Common dimensions: w=1920&h=1080 (hero), w=800&h=600 (cards), w=400&h=400 (thumbnails)

2. **AI-generated images via image-generation skill**:
   - Use when stock photos don't fit the vision
   - Specify dimensions (16:9 for hero, 1:1 for cards, 9:16 for mobile)
   - Match design tokens with `--include-css-vars` flag
   - Define style (glassmorphism, minimal, photorealistic, abstract)

3. **Custom graphics**:
   - SVG patterns, CSS gradients, geometric shapes
   - Noise textures, mesh gradients, generative art

**Quality Standards**:
- Minimum 1200px width for hero, 800px for features
- Optimize file size <500KB per image
- Include descriptive alt text for ALL images
- Images must align with the aesthetic direction
- Use consistent visual style throughout

**Background Requirements**:
Always add visual depth—gradients, noise overlays, patterns, or textured images. Never use flat solid colors alone.