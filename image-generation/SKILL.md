---
name: image-generation
description: Generate high-quality frontend assets using Google Nano Banana Pro API. Specialized in UI/UX assets including backgrounds, logos, icons, illustrations, and design system components. Automatically infers context from project design tokens, CSS variables, and component requirements. Supports aspect ratios (1:1, 16:9, 9:16, 21:9, custom), resolutions (1K-4K), style presets, and frontend-specific parameters. Use when creating visual assets for web applications, design systems, or marketing materials.
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
---

# Image Generation (Nano Banana Pro)

Specialized skill for generating premium frontend and web assets using **Google Nano Banana Pro** (Gemini 3 Pro Image). Optimized for UI/UX designers, frontend developers, and design system workflows.

## Quick Start

### Prerequisites

**API Key Setup**: The skill checks for `GEMINI_API_KEY` in this order:
1. Process environment: `export GEMINI_API_KEY="your-key"`
2. Project root: `.env`
3. `.claude/.env`
4. `.claude/skills/.env`
5. `.claude/skills/image-generation/.env`

**Get API key**: https://aistudio.google.com/apikey

**Install SDK** (for Python integration):
```bash
pip install google-genai python-dotenv pillow
```

## Model Selection

### Nano Banana Pro (gemini-3-pro-image-preview)
**Best for**: Production-grade assets, complex text rendering, logos, icons with text.

- **Quality**: Highest fidelity, professional output
- **Strengths**: Text rendering, complex compositions, brand assets
- **Resolution**: Up to 4K
- **Use cases**: Logos, marketing graphics, hero images, UI illustrations
- **Speed**: ~10 seconds per generation
- **Token cost**: 4K resolution = 4K tokens

### Nano Banana (gemini-2.5-flash-image)
**Best for**: Rapid prototyping, simple icons, concept exploration.

- **Quality**: Good for drafts and iterations
- **Strengths**: Fast generation, simple compositions
- **Resolution**: Up to 2K
- **Use cases**: Wireframe visuals, icon concepts, background variations
- **Speed**: ~3-5 seconds per generation
- **Token cost**: 2K resolution = 2K tokens

## Supported Parameters

### Aspect Ratios
```bash
--aspect-ratio 1:1     # Square (icons, logos, thumbnails)
--aspect-ratio 16:9    # Widescreen (hero banners, presentations)
--aspect-ratio 9:16    # Portrait (mobile stories, social media)
--aspect-ratio 21:9    # Ultrawide (cinematic, panoramic)
--aspect-ratio 4:3     # Standard (documents, tablets)
--aspect-ratio 3:2     # Photo format (product photography)
--aspect-ratio custom  # Specify custom dimensions
```

### Resolutions
```bash
--resolution 1K   # 1024x1024 (fast, good for icons)
--resolution 2K   # 2048x2048 (balanced, good for UI elements)
--resolution 4K   # 4096x4096 (premium, best for print/marketing)
```

### Style Presets
```bash
--style flat-minimalist       # Clean lines, solid colors, no gradients
--style glassmorphism         # Frosted glass, blur, transparency
--style neo-brutalism         # Bold colors, thick borders, raw aesthetic
--style photorealistic        # Realistic lighting, textures, shadows
--style 3D-render            # Depth, dimension, studio lighting
--style abstract-geometric    # Shapes, patterns, geometric compositions
--style isometric            # 2.5D perspective, grid-based
--style hand-drawn           # Sketch, illustration, organic feel
--style tech-cyberpunk       # Neon, dark mode, futuristic
--style vintage-retro        # Nostalgic textures, muted colors
```

### Frontend-Specific Parameters
```bash
--format png                 # PNG (recommended for UI assets with transparency)
--format jpeg                # JPEG (smaller file size, no transparency)
--format webp                # WebP (modern, excellent compression)

--background transparent     # Transparent background (PNG only)
--background solid           # Solid color background
--background gradient        # Gradient background

--include-css-vars           # Auto-detect and use CSS variables from project
--match-design-tokens        # Extract colors, spacing, typography from design system
--infer-from-context         # Analyze component usage to infer optimal style
```

## Context Awareness

### Automatic Design Token Detection
When `--include-css-vars` is enabled, the skill automatically:

1. **Scans project files** for CSS variables:
   - `**/*.css` for CSS custom properties
   - `**/tailwind.config.js` for Tailwind theme
   - `**/globals.css` for design tokens
   - `**/design-tokens.*` for design system files

2. **Extracts color palette**:
   - Primary, secondary, accent colors
   - Background, surface, border colors
   - Text, link, muted colors

3. **Detects typography**:
   - Font families, weights, sizes
   - Line heights, letter spacing
   - Heading hierarchies

4. **Identifies spacing system**:
   - Border radius values
   - Spacing scale (4px, 8px, 16px, etc.)
   - Shadow definitions

5. **Injects into prompts** automatically:
   ```
   "Modern hero section background using brand colors #0F172A (dark blue) and
   #F59E0B (amber), 16:9 aspect ratio, glassmorphism style with 8px border radius,
   consistent with Inter font family, 4K resolution"
   ```

### Component Context Inference
When `--infer-from-context` is enabled:

1. **Analyzes component type** (button, card, navbar, etc.)
2. **Determines optimal aspect ratio** based on usage
3. **Selects appropriate resolution** based on display size
4. **Recommends style preset** based on design system
5. **Generates multiple variants** for different states (hover, active, disabled)

## Asset Recipes

### Website Backgrounds

**Hero Section Backgrounds** (16:9 or 21:9):
```bash
# Abstract gradients
"Abstract gradient background with [color1] and [color2], soft blur, 16:9, 4K"

# Glassmorphism
"Frosted glass effect with subtle gradient, depth blur, light reflections, 21:9, 4K"

# Geometric patterns
"Minimal geometric pattern, thin lines, [color] accents, plenty of white space, 16:9, 2K"

# Dark mode
"Dark abstract background, deep [color] gradients, subtle glow effects, high contrast, 16:9, 4K"
```

**Section Backgrounds** (16:9 or custom):
```bash
# Subtle textures
"Subtle noise texture, light gray gradient, clean minimalist, 16:9, 2K"

# Organic shapes
"Organic blob shapes, pastel colors, soft gradients, playful, 16:9, 2K"

# Grid patterns
"Subtle grid pattern, technical aesthetic, thin lines, professional, 16:9, 2K"
```

### Logos & Brand Assets

**Minimalist Logos** (1:1 or custom):
```bash
# Wordmark
"Clean typographic logo for [company], [font] style, [color], minimal, professional, 1:1, 4K"

# Icon + text
"Simple icon logomark with [industry] symbol, [company] text below, [color1] and [color2], modern, 1:1, 4K"

# Lettermark
"Stylized lettermark using [initials], geometric construction, [color], bold, memorable, 1:1, 4K"

# Emblem
"Circular emblem logo, [icon] in center, [company] text curved around border, [color1], [color2], classic, 1:1, 4K"
```

**Logo Variations**:
```bash
# Light version
"[logo description], white on transparent background, minimal"

# Dark version
"[logo description], black on transparent background, inverted"

# Monochrome
"[logo description], single color [hex], grayscale, versatile"

# Favicon version
"[logo icon only], 32x32px simplified, high contrast, recognizable at small size"
```

### UI Icons

**3D Glassmorphism Icons** (1:1):
```bash
"3D glassmorphism [icon] icon, semi-transparent, refractive, soft studio lighting,
[brand-color] accent, white background, 1:1, 2K"
```

**Flat Line Icons** (1:1):
```bash
"Minimal line art [icon] icon, 2px stroke, [color], clean edges, white background,
vector-style, 1:1, 1K"
```

**Filled Glyph Icons** (1:1):
```bash
"Solid glyph [icon] icon, filled shape, [color], flat design, modern app style,
white background, 1:1, 1K"
```

**Isometric Icons** (1:1):
```bash
"Isometric 2.5D [icon] icon, 45-degree angle, subtle shadow, [color1] and [color2],
soft lighting, 1:1, 2K"
```

### Illustrations

**Hero Illustrations** (16:9 or 21:9):
```bash
# Flat illustration
"Flat vector illustration of [scene], [style] art style, [color-palette], clean shapes,
no gradients, 16:9, 4K"

# 3D illustration
"3D rendered illustration of [scene], soft lighting, clay material, [colors],
playful, 16:9, 4K"

# Hand-drawn illustration
"Hand-drawn sketch style illustration of [scene], ink lines, subtle watercolor,
organic feel, 16:9, 2K"
```

**Empty State Illustrations** (1:1 or 16:9):
```bash
# Minimal
"Minimal [concept] illustration, simple shapes, muted colors, clean, 1:1, 2K"

# Friendly
"Friendly character illustration, [emotion], soft colors, approachable, 1:1, 2K"

# Technical
"Technical diagram showing [concept], clean lines, annotations, professional, 16:9, 2K"
```

### Marketing & Social Media

**Social Media Posts** (1:1 or 9:16):
```bash
# Instagram square
"Instagram post graphic, [topic], [brand-colors], bold text overlay, engaging,
high contrast, 1:1, 2K"

# Instagram story
"Instagram story background, vertical, [topic], [brand-colors], modern, gradient,
9:16, 2K"

# Twitter post
"Twitter post graphic, [announcement], [brand-colors], minimal text, shareable, 16:9, 2K"
```

**Blog Thumbnails** (16:9):
```bash
"Blog post thumbnail, [topic], professional, [brand-colors], clear title space,
engaging, 16:9, 2K"
```

**Email Headers** (custom ratio):
```bash
"Email header graphic, [campaign], [brand-colors], on-brand, 600px width, mobile-optimized, 2K"
```

## Direct API Access (CURL)

### Basic Image Generation

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [{
      "parts": [{
        "text": "Abstract glassmorphism background with soft amber and stone gradients, 4K, shallow depth of field, premium feel."
      }]
    }]
  }'
```

### Generation with Aspect Ratio

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [{
      "parts": [{
        "text": "Modern hero section background using brand colors #0F172A and #F59E0B, 16:9 aspect ratio, glassmorphism style, 4K"
      }]
    }]
  }'
```

### Generation with Negative Prompting

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [{
      "parts": [{
        "text": "Minimalist logo for tech startup, geometric, blue and white, clean. NO: text, realistic, photorealistic, 3D, shadows, gradients"
      }]
    }]
  }'
```

## Integration with Other Skills

### Aesthetic Skill Integration

When used with the **aesthetic** skill:

1. **Extract design tokens** from CSS variables and Tailwind config
2. **Generate design images** matching extracted colors and typography
3. **Evaluate aesthetic quality** through iterative analysis
4. **Refine prompts** based on aesthetic feedback

**Example workflow**:
```markdown
1. Use aesthetic skill to analyze inspiration sites
2. Extract color palette and typography
3. Use image-generation skill with --include-css-vars
4. Generate background variations
5. Use aesthetic skill to evaluate quality
6. Iterate until score ≥ 7/10
```

### Frontend Development Integration

When used with **ui-styling** and **web-frameworks** skills:

1. **Generate component assets** matching shadcn/ui design system
2. **Create themed backgrounds** for different pages
3. **Design icons** that match component aesthetics
4. **Produce marketing assets** consistent with app design

**Example workflow**:
```markdown
1. Use ui-styling to set up shadcn/ui with theme
2. Use image-generation with --match-design-tokens
3. Generate hero background for landing page
4. Generate logo variations for different contexts
5. Generate icon set matching design system
```

### Media Processing Integration

When used with **media-processing** skill:

1. **Generate base image** with Nano Banana Pro
2. **Convert format** (PNG → WebP for better compression)
3. **Resize for multiple contexts** (1K, 2K, 4K variants)
4. **Apply filters** (blur, sharpen, color adjust)
5. **Create sprite sheets** for icons

## Advanced Techniques

### Text Rendering Prompts

Nano Banana Pro excels at text rendering. Use these techniques:

```bash
# Clear text with contrast
"Modern headline 'Welcome' in bold, sans-serif font, dark on light background,
high contrast, readable, professional"

# Text in specific style
"Retro-style headline 'SALE' with gradient text, bubble letters, colorful, playful"

# Text overlay on image
"Landscape background with overlay text 'Adventure Awaits', white text with
shadow, cinematic, centered"
```

### Consistency Across Assets

To maintain visual consistency:

```bash
# 1. Define base style parameters
BASE_STYLE="minimal, geometric, brand colors #3B82F6 and #10B981, clean"

# 2. Generate variations with consistent base
"${BASE_STYLE} logo icon, 1:1"
"${BASE_STYLE} hero background, 16:9"
"${BASE_STYLE} social media post, 1:1"

# 3. Use same color palette and style modifiers
```

### Iterative Refinement

```bash
# Generation 1: Base concept
"Modern tech logo, blue, geometric"

# Generation 2: Add details
"Modern tech logo, blue hexagon shape, circuit patterns, minimalist"

# Generation 3: Refine quality
"Minimalist hexagon logo with circuit patterns, royal blue #2563EB, clean 2px stroke,
white background, vector style, professional"

# Generation 4: Final polish
"Minimalist hexagon logomark, royal blue #2563EB, 2px stroke, circuit patterns
interwoven, ample white space, clean edges, professional tech company, 1:1, 4K"
```

## Output Handling

### Saving Generated Images

The API returns Base64-encoded image data in:
```json
{
  "candidates": [{
    "content": {
      "parts": [{
        "inlineData": {
          "data": "iVBORw0KGgoAAAANSUhEUgAA...",
          "mimeType": "image/png"
        }
      }]
    }
  }]
}
```

**Save to file** (Python):
```python
import base64
import json

# Extract from API response
image_data = response['candidates'][0]['content']['parts'][0]['inlineData']['data']
image_bytes = base64.b64decode(image_data)

# Save to file
with open('output.png', 'wb') as f:
    f.write(image_bytes)
```

**Save to file** (Bash):
```bash
# If using a script that outputs JSON
echo '{"image": "base64data..."}' | jq -r '.image' | base64 -d > output.png
```

### Batch Generation

Generate multiple assets in parallel:

```bash
# Create list of prompts
prompts=(
  "Hero background 1"
  "Hero background 2"
  "Logo variation 1"
  "Logo variation 2"
)

# Generate in parallel
for prompt in "${prompts[@]}"; do
  curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent?key=$GEMINI_API_KEY" \
    -H "Content-Type: application/json" \
    -X POST \
    -d "{\"contents\":[{\"parts\":[{\"text\": \"$prompt\"}]}]}" \
    -o "output_$(date +%s).json" &
done

wait
echo "All generations complete"
```



**Best practices**:
- Implement exponential backoff for rate limit errors
- Cache generated assets to avoid regeneration
- Use Flash model for iterations, Pro for final output
- Batch process multiple requests with delays



### Quality Issues

**Blurry output**:
- Increase resolution (use 2K or 4K)
- Add "sharp focus" to prompt
- Specify "high detail" or "ultra detailed"

**Inconsistent style**:
- Define consistent base style parameters
- Use same color palette across generations
- Reference previous successful prompts

**Text rendering issues**:
- Specify font style (sans-serif, serif, etc.)
- Add contrast requirements
- Mention "legible" or "clear text"
- Try simpler text first, then add complexity

## Best Practices

### Prompt Engineering

1. **Be specific**: "Blue" → "Royal blue #2563EB"
2. **Order matters**: Subject → Style → Technical specs
3. **Use negatives**: What to avoid is as important as what to include
4. **Iterate**: Start simple, add complexity gradually
5. **Reference style**: Mention known styles or brands as anchors

### Frontend Workflow

1. **Start with design tokens**: Extract before generating
2. **Generate variations**: Create multiple options to choose from
3. **Test in context**: View generated assets in actual UI
4. **Optimize formats**: Use WebP for production, PNG for transparency needed
5. **Version control**: Keep prompts alongside generated assets
6. **Document decisions**: Save rationale for design choices

### Asset Organization

```
project-root/
├── docs/
│   └── assets/
│       ├── backgrounds/
│       │   ├── hero-section.png
│       │   └── section-gradient.png
│       ├── logos/
│       │   ├── primary-logo.png
│       │   ├── favicon.png
│       │   └── social-avatar.png
│       ├── icons/
│       │   ├── nav-home.png
│       │   ├── nav-profile.png
│       │   └── feature-icons.png
│       └── prompts/
│           ├── hero-background-prompt.md
│           └── logo-variations-prompt.md
```

