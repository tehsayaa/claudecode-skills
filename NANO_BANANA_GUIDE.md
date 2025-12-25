# Google Nano Banana Image Generation - Complete Guide

Complete guide to using Google's **Nano Banana** image generation models in your applications.

## 📋 Table of Contents
- [Overview](#overview)
- [Model Options](#model-options)
- [Setup](#setup)
- [API Usage](#api-usage)
- [Code Examples](#code-examples)
- [Response Handling](#response-handling)
- [Best Practices](#best-practices)
- [Error Handling](#error-handling)
- [Common Issues](#common-issues)

---

## 🎯 Overview

**Nano Banana** is Google's native image generation capability available through the Gemini API. It consists of two models:

- **Nano Banana** (`gemini-2.5-flash-image`): Fast, optimized for high-volume and low-latency
- **Nano Banana Pro** (`gemini-3-pro-image-preview`): Professional quality with advanced reasoning

### Key Characteristics

| Feature | Nano Banana | Nano Banana Pro |
|---------|-------------|-----------------|
| **Speed** | ⚡ 3-4 seconds | 🐢 8-12 seconds |
| **Quality** | Good | Excellent |
| **Reasoning** | Basic | Advanced ("Thinking") |
| **Text Rendering** | Basic | High-fidelity |
| **Best For** | Iterations, volume | Professional assets |

---

## 🚀 Model Options

### Nano Banana (Fast)
```javascript
model: 'gemini-2.5-flash-image'
```
- **Generation Time**: ~3-4 seconds
- **Cost**: ~$0.039 per image
- **Use Case**: Quick iterations, prototypes, high-volume generation

### Nano Banana Pro (HQ)
```javascript
model: 'gemini-3-pro-image-preview'
```
- **Generation Time**: ~8-12 seconds
- **Cost**: Higher than standard
- **Use Case**: Professional assets, complex scenes, text-heavy images

---

## 📦 Setup

### 1. Install the SDK

```bash
npm install @google/generative-ai
```

### 2. Get API Key

1. Go to [Google AI Studio](https://aistudio.google.com/)
2. Sign in with your Google account
3. Click "Get API Key"
4. Copy your API key

### 3. Set Environment Variable

```bash
# .env.local
GOOGLE_AI_API_KEY=your_api_key_here
```

**⚠️ Security**: Never commit `.env.local` to version control!

---

## 🔧 API Usage

### Basic Setup

```javascript
import { GoogleGenerativeAI } from '@google/generative-ai';

// Initialize with API key
const genAI = new GoogleGenerativeAI(process.env.GOOGLE_AI_API_KEY);
```

### Get the Model

```javascript
// For fast generation
const model = genAI.getGenerativeModel({
  model: 'gemini-2.5-flash-image'
});

// For high-quality generation
const model = genAI.getGenerativeModel({
  model: 'gemini-3-pro-image-preview'
});
```

### Generate Image

```javascript
// Simple text prompt
const result = await model.generateContent(
  'A magical forest with glowing mushrooms'
);

// The response contains the generated image
const response = result.response;
```

---

## 💻 Code Examples

### Example 1: Basic Image Generation (Node.js)

```javascript
import { GoogleGenerativeAI } from '@google/generative-ai';
import * as fs from 'node:fs';

const genAI = new GoogleGenerativeAI(process.env.GOOGLE_AI_API_KEY);

async function generateImage(prompt) {
  // Get the model
  const model = genAI.getGenerativeModel({
    model: 'gemini-2.5-flash-image'
  });

  // Generate the image
  const result = await model.generateContent(prompt);

  // Extract image data
  const candidates = result.response.candidates || [];
  if (candidates.length > 0) {
    const parts = candidates[0].content?.parts || [];

    for (const part of parts) {
      if (part.inlineData) {
        // Get base64 image data
        const imageData = part.inlineData.data;
        const mimeType = part.inlineData.mimeType || 'image/png';

        // Convert to buffer
        const buffer = Buffer.from(imageData, 'base64');

        // Save to file
        fs.writeFileSync('generated-image.png', buffer);

        console.log('Image saved as generated-image.png');
        return `data:${mimeType};base64,${imageData}`;
      }
    }
  }
}

// Usage
generateImage('A futuristic city with flying cars');
```

### Example 2: Next.js API Route

```javascript
// app/api/generate/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { GoogleGenerativeAI } from '@google/generative-ai';

const genAI = new GoogleGenerativeAI(process.env.GOOGLE_AI_API_KEY!);

export async function POST(req: NextRequest) {
  try {
    const { prompt, model = 'gemini-2.5-flash-image' } = await req.json();

    if (!prompt) {
      return NextResponse.json(
        { error: 'Prompt is required' },
        { status: 400 }
      );
    }

    // Get the model
    const aiModel = genAI.getGenerativeModel({ model });

    // Generate content
    const result = await aiModel.generateContent(prompt);

    // Extract image from response
    const candidates = result.response.candidates || [];
    if (candidates.length > 0) {
      const parts = candidates[0].content?.parts || [];

      for (const part of parts) {
        if (part.inlineData) {
          const imageData = part.inlineData.data;
          const mimeType = part.inlineData.mimeType || 'image/png';

          // Return base64 data URL
          return NextResponse.json({
            success: true,
            data: `data:${mimeType};base64,${imageData}`,
          });
        }
      }
    }

    return NextResponse.json(
      { error: 'No image generated' },
      { status: 500 }
    );
  } catch (error: any) {
    console.error('Image generation error:', error);
    return NextResponse.json(
      { error: error.message },
      { status: 500 }
    );
  }
}
```

### Example 3: React Component

```javascript
'use client';

import { useState } from 'react';

export default function ImageGenerator() {
  const [prompt, setPrompt] = useState('');
  const [imageUrl, setImageUrl] = useState('');
  const [loading, setLoading] = useState(false);

  const generateImage = async () => {
    setLoading(true);
    try {
      const response = await fetch('/api/generate', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          prompt: prompt,
          model: 'gemini-2.5-flash-image',
        }),
      });

      const data = await response.json();
      if (data.success) {
        setImageUrl(data.data);
      }
    } catch (error) {
      console.error('Error:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div>
      <input
        value={prompt}
        onChange={(e) => setPrompt(e.target.value)}
        placeholder="Enter your prompt..."
      />
      <button onClick={generateImage} disabled={loading}>
        {loading ? 'Generating...' : 'Generate'}
      </button>

      {imageUrl && (
        <img src={imageUrl} alt="Generated image" />
      )}
    </div>
  );
}
```

### Example 4: REST API Call

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-image:generateContent" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H 'Content-Type: application/json' \
  -X POST \
  -d '{
    "contents": [{
      "parts": [{
        "text": "Create a picture of a futuristic banana with neon lights in a cyberpunk city."
      }]
    }]
  }'
```

---

## 📦 Response Handling

### Response Structure

```javascript
{
  response: {
    candidates: [
      {
        content: {
          parts: [
            {
              text: "Optional text description",
              inlineData: {
                data: "base64_encoded_image_data",
                mimeType: "image/png"
              }
            }
          ]
        }
      }
    ]
  }
}
```

### Extracting the Image

```javascript
const candidates = result.response.candidates || [];

if (candidates.length > 0) {
  const parts = candidates[0].content?.parts || [];

  for (const part of parts) {
    // Check if this part contains an image
    if (part.inlineData) {
      const imageData = part.inlineData.data;
      const mimeType = part.inlineData.mimeType || 'image/png';

      // Create a data URL for display in browsers
      const dataUrl = `data:${mimeType};base64,${imageData}`;

      // Or convert to buffer for Node.js
      const buffer = Buffer.from(imageData, 'base64');

      return { dataUrl, buffer, mimeType };
    }
  }
}
```

---

## ✨ Best Practices

### 1. Prompt Engineering

```javascript
// Good prompts are specific and descriptive
const goodPrompt = "A serene Japanese garden at sunset, cherry blossoms falling, koi fish pond, traditional wooden bridge, Mount Fuji in the background, cinematic lighting, 4K quality";

// Bad prompts are too vague
const badPrompt = "a garden";
```

### 2. Model Selection

```javascript
// Use Nano Banana for:
- Quick iterations
- Testing concepts
- High-volume generation
- Real-time applications

// Use Nano Banana Pro for:
- Final production assets
- Complex scenes
- Images with text
- Professional work
```

### 3. Error Handling

```javascript
try {
  const result = await model.generateContent(prompt);

  // Always check candidates array
  if (!result.response.candidates || result.response.candidates.length === 0) {
    throw new Error('No candidates in response');
  }

  // Check for finish reason
  const candidate = result.response.candidates[0];
  if (candidate.finishReason === 'SAFETY') {
    throw new Error('Content blocked by safety filters');
  }

} catch (error) {
  console.error('Generation failed:', error);
}
```

### 4. Rate Limiting

```javascript
// Implement delays between requests
async function generateWithDelay(prompt) {
  await new Promise(resolve => setTimeout(resolve, 1000));
  return await model.generateContent(prompt);
}
```

---

## ⚠️ Error Handling

### Common Errors

```javascript
// 1. Invalid API Key
{
  "error": "API key not valid. Please pass a valid API key."
}

// 2. Model Not Found
{
  "error": "models/gemini-2.5-flash-image is not found"
}

// 3. Safety Filter
{
  "finishReason": "SAFETY",
  "safetyRatings": [...]
}

// 4. Quota Exceeded
{
  "error": "Quota exceeded"
}
```

### Handling Errors Gracefully

```javascript
async function safeGenerateImage(prompt) {
  try {
    const result = await model.generateContent(prompt);

    // Check for safety issues
    const candidate = result.response.candidates?.[0];
    if (candidate?.finishReason === 'SAFETY') {
      return {
        success: false,
        error: 'Content blocked by safety guidelines',
        safe: false
      };
    }

    // Extract and return image
    const image = extractImage(result);
    return { success: true, data: image };

  } catch (error) {
    if (error.message.includes('API key')) {
      return { success: false, error: 'Invalid API key' };
    }
    if (error.message.includes('Quota')) {
      return { success: false, error: 'Rate limit exceeded' };
    }
    return { success: false, error: 'Generation failed' };
  }
}
```

---

## 🔧 Common Issues & Solutions

### Issue 1: No Image in Response

**Problem**: Response contains text but no image

**Solution**:
```javascript
// Check all parts in the response
for (const part of parts) {
  if (part.inlineData) {
    // Found the image!
  }
}
```

### Issue 2: CORS Errors (Browser)

**Problem**: Calling API directly from browser fails

**Solution**: Always use a backend API route
```javascript
// ✅ Good: Use API route
fetch('/api/generate', { ... })

// ❌ Bad: Call API directly from browser
fetch('https://generativelanguage.googleapis.com/...', { ... })
```

### Issue 3: Large Image Size

**Problem**: Base64 images are very large

**Solution**: Compress or resize
```javascript
// Resize before displaying
<img src={imageUrl} style={{ maxWidth: '512px' }} />
```

### Issue 4: Slow Generation

**Problem**: Taking too long to generate

**Solution**:
```javascript
// Use faster model
model: 'gemini-2.5-flash-image'  // Instead of gemini-3-pro-image-preview

// Add timeout
const controller = new AbortController();
setTimeout(() => controller.abort(), 30000); // 30s timeout
```

---

## 📚 Additional Resources

- [Official Documentation](https://ai.google.dev/gemini-api/docs/nanobanana)
- [Google AI Studio](https://aistudio.google.com/)
- [Model Reference](https://ai.google.dev/gemini-api/docs/models)
- [Pricing](https://ai.google.dev/pricing)

---

## 🎯 Quick Reference

### Initialize
```javascript
const genAI = new GoogleGenerativeAI(apiKey);
const model = genAI.getGenerativeModel({ model: 'gemini-2.5-flash-image' });
```

### Generate
```javascript
const result = await model.generateContent('Your prompt here');
```

### Extract
```javascript
const imageData = result.response.candidates[0].content.parts[0].inlineData.data;
const dataUrl = `data:image/png;base64,${imageData}`;
```

---

## 📝 Summary

1. **Choose your model**: Fast (2.5-flash) or Quality (3-pro)
2. **Initialize**: `new GoogleGenerativeAI(apiKey)`
3. **Generate**: `model.generateContent(prompt)`
4. **Extract**: Get `inlineData.data` from response parts
5. **Display**: Convert to data URL or save as file

Happy generating! 🎨✨
