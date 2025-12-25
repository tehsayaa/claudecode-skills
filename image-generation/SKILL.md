---
name: image-generation
description: High-fidelity image generation for frontend assets using Google Nano Banana Pro.
---

# Image Generation (Nano Banana Pro)

Dedicated skill for generating premium web assets using the **Google Nano Banana** model suite.

##  Direct API Access (CURL)

Use these commands to generate images directly. Base64 data is returned in the `inlineData` part of the response.

###  Nano Banana Pro (gemini-3-pro-image-preview)
Best for: Professional asset production, complex text rendering, and advanced visual reasoning.

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

###  Nano Banana (gemini-2.5-flash-image)
Best for: Rapid prototyping, simple icons, and low-latency tasks.

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-image:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [{
      "parts": [{
        "text": "Minimal sleek line art logo for a biotech clinic, emerald green on white background."
      }]
    }]
  }'
```

##  Asset Recipes
- **Website Backgrounds**: *"Abstract glassmorphism, stone/cream gradients, amber shards, 85mm, f1.4, 4K."*
- **Sleek Logos**: *"Minimal vector logomark, high contrast, clean edges, commercial aesthetic."*
- **UI Icons**: *"3D glassmorphism icon, semi-transparent, refractive, soft studio lighting."*

##  Technical Notes
- **API Key**: Loaded from `.env` as `GEMINI_API_KEY`.
- **Response**: The image data is in `candidates[0].content.parts[0].inlineData.data` as a Base64 string.
- **Limits**: Nano Banana Pro (Gemini 3 Pro) allows for complex multi-step "Thinking" prompts.
