# Z.ai MCP Server Migration Guide

**Date**: 2025-12-30
**Purpose**: Migrate from Gemini API to Z.ai MCP servers for vision, OCR, and web tasks
**Status**: Ready for Implementation

---

## Overview

This guide provides a complete migration path from Google Gemini API to Z.ai MCP servers for specific tasks. Z.ai MCP servers offer specialized, high-performance tools for:

- **Vision & OCR** (Vision MCP Server)
- **Web Search** (Search MCP Server)
- **Web Content Reading** (Reader MCP Server)

---

## Available Z.ai MCP Tools

### 1. Vision MCP Server Tools

**Server**: `mcp__zai-mcp-server`

| Tool | Purpose | Use When |
|------|---------|----------|
| `extract_text_from_screenshot` | OCR for code, terminals, docs | Extracting text from images |
| `diagnose_error_screenshot` | Analyze errors & propose fixes | Debugging error messages |
| `understand_technical_diagram` | Architecture, UML, ER diagrams | Understanding technical docs |
| `analyze_data_visualization` | Charts, graphs, dashboards | Data analysis from images |
| `ui_diff_check` | Compare two UI screenshots | QA, testing design consistency |
| `ui_to_artifact` | Convert UI to code/prompts/specs | Frontend development |
| `analyze_image` | General image understanding | Captions, descriptions |
| `analyze_video` | Video scene analysis | Understanding video content |

### 2. Search MCP Server Tools

**Server**: `mcp__web-search-prime`

| Tool | Purpose | Parameters |
|------|---------|------------|
| `webSearchPrime` | Web search with rich results | query, domain_filter, recency_filter, content_size, location |

### 3. Reader MCP Server Tools

**Server**: `mcp__web-reader` or `mcp__web_reader`

| Tool | Purpose | Parameters |
|------|---------|------------|
| `webReader` | Fetch & convert URLs to LLM-friendly input | url, timeout, return_format, retain_images |

---

## Migration Mapping

### Gemini → Z.ai MCP Equivalents

| Gemini Task | Gemini API | Z.ai MCP Tool | Notes |
|-------------|------------|---------------|-------|
| **Image Understanding** | `gemini-2.5-flash` vision | `analyze_image` | General purpose |
| **OCR (Text Extraction)** | `gemini-2.5-flash` vision | `extract_text_from_screenshot` | Better for code/terminals |
| **Error Analysis** | Manual analysis | `diagnose_error_screenshot` | Specialized for errors |
| **Diagram Understanding** | `gemini-2.5-flash` vision | `understand_technical_diagram` | Better for technical diagrams |
| **Data Visualization** | Manual analysis | `analyze_data_visualization` | Specialized for charts |
| **UI Comparison** | Manual comparison | `ui_diff_check` | Automated comparison |
| **UI to Code** | Manual conversion | `ui_to_artifact` | Code generation |
| **Web Search** | Google Search | `webSearchPrime` | Richer results |
| **Web Content** | Manual fetch/read | `webReader` | Optimized for LLMs |

---

## Skills Requiring Updates

### Priority 1: High Impact

#### 1. **ai-multimodal** Skill
**Current Usage**:
- Image understanding (captions, descriptions)
- OCR and text extraction
- Video analysis

**Migration Plan**:
```markdown
BEFORE:
- Use Gemini 2.5 Flash for vision
- Use Gemini 2.5 Flash for OCR

AFTER:
- Use Z.ai `analyze_image` for general image understanding
- Use Z.ai `extract_text_from_screenshot` for OCR
- Use Z.ai `analyze_video` for video content
- Keep Gemini for audio, document processing (no Z.ai equivalent)
```

**Benefits**:
- ✅ Better OCR accuracy for code/terminals
- ✅ Specialized tools for specific tasks
- ✅ No API key management (MCP handles it)
- ✅ Faster processing for specialized tasks

#### 2. **aesthetic** Skill
**Current Usage**:
- Screenshot analysis via ai-multimodal
- Design token extraction
- Aesthetic quality evaluation

**Migration Plan**:
```markdown
BEFORE:
- Use ai-multimodal → Gemini for screenshot analysis

AFTER:
- Use Z.ai `analyze_image` for general screenshots
- Use Z.ai `ui_diff_check` for design comparisons
- Use Z.ai `extract_text_from_screenshot` for extracting typography/labels
- Keep ai-multimodal for document extraction (PDFs, charts)
```

**Benefits**:
- ✅ Better UI-focused understanding
- ✅ Automated design consistency checks
- ✅ Specialized screenshot analysis

#### 3. **docs-seeker** Skill
**Current Usage**:
- Web search for documentation

**Migration Plan**:
```markdown
BEFORE:
- Use Google Search or built-in search

AFTER:
- Use Z.ai `webSearchPrime` for all web searches
- Leverage rich results (titles, summaries, icons)
- Use domain filtering for specific documentation sites
```

**Benefits**:
- ✅ Richer search results
- ✅ Domain filtering (official docs only)
- ✅ Recency filtering (latest docs)
- ✅ Better content summaries

### Priority 2: Medium Impact

#### 4. **document-skills/pdf** Skill
**Current Usage**:
- Text extraction from PDFs

**Migration Plan**:
```markdown
BEFORE:
- Use Gemini for PDF understanding

AFTER:
- Keep Gemini for PDF vision processing (no Z.ai equivalent yet)
- Use Z.ai `extract_text_from_screenshot` if PDF is converted to images first
```

#### 5. **document-skills/pptx** Skill
**Current Usage**:
- PowerPoint content extraction

**Migration Plan**:
```markdown
BEFORE:
- Use Gemini for slide analysis

AFTER:
- Keep Gemini for presentation analysis
- Use Z.ai `extract_text_from_screenshot` for individual slide screenshots
```

---

## Implementation Examples

### Example 1: OCR with Z.ai MCP

**Before (Gemini)**:
```python
# Using Gemini API
response = genai.generate_content(
    model="gemini-2.5-flash",
    contents=["Extract text from this image", image_blob]
)
text = response.text
```

**After (Z.ai MCP)**:
```python
# Using Z.ai MCP tool
from mcp import ClientSession

session = ClientSession()
result = await session.call_tool(
    "mcp__zai-mcp-server",
    "extract_text_from_screenshot",
    {
        "image_source": "/path/to/screenshot.png",
        "prompt": "Extract all text from this screenshot",
        "programming_language": "python"  # For code screenshots
    }
)
text = result.content[0].text
```

### Example 2: Web Search with Z.ai MCP

**Before (Google Search)**:
```python
# Manual web search or using search API
results = search_api.query("Next.js 15 documentation")
```

**After (Z.ai MCP)**:
```python
# Using Z.ai MCP tool
result = await session.call_tool(
    "mcp__web-search-prime",
    "webSearchPrime",
    {
        "search_query": "Next.js 15 App Router documentation",
        "search_domain_filter": "nextjs.org",  # Official docs only
        "search_recency_filter": "oneMonth",  # Recent docs
        "content_size": "high",  # Detailed summaries
        "location": "us"
    }
)
```

### Example 3: Screenshot Analysis with Z.ai MCP

**Before (Gemini)**:
```python
response = genai.generate_content(
    model="gemini-2.5-flash",
    contents=["Analyze this UI screenshot", image_blob]
)
analysis = response.text
```

**After (Z.ai MCP)**:
```python
# For general understanding
result = await session.call_tool(
    "mcp__zai-mcp-server",
    "analyze_image",
    {
        "image_source": "/path/to/screenshot.png",
        "prompt": "Describe the layout, colors, typography, and components in this UI"
    }
)

# For UI comparison
result = await session.call_tool(
    "mcp__zai-mcp-server",
    "ui_diff_check",
    {
        "expected_image_source": "/path/to/design.png",
        "actual_image_source": "/path/to/implementation.png",
        "prompt": "Compare these two UIs and identify differences"
    }
)
```

---

## Skill Update Instructions

### For ai-multimodal Skill

1. **Update Description**:
```yaml
description: Process audio, video, documents using Google Gemini API. Use Z.ai MCP tools for image understanding, OCR, and specialized vision tasks.
```

2. **Add Tool References**:
```yaml
allowed-tools:
  - mcp__zai-mcp-server__analyze_image
  - mcp__zai-mcp-server__extract_text_from_screenshot
  - mcp__zai-mcp-server__diagnose_error_screenshot
  - mcp__zai-mcp-server__understand_technical_diagram
  - mcp__zai-mcp-server__analyze_data_visualization
  - mcp__zai-mcp-server__ui_diff_check
  - mcp__zai-mcp-server__ui_to_artifact
  - mcp__zai-mcp-server__analyze_video
```

3. **Update Quick Start**:
```markdown
### Image Understanding
- **Redirect**: Use Z.ai MCP Vision tools via mcp__zai-mcp-server
  - General understanding: `analyze_image`
  - OCR/text extraction: `extract_text_from_screenshot`
  - Error diagnosis: `diagnose_error_screenshot`
  - Technical diagrams: `understand_technical_diagram`
  - Data visualizations: `analyze_data_visualization`
  - UI comparison: `ui_diff_check`
  - UI to code: `ui_to_artifact`

### Video Analysis
- **Redirect**: Use Z.ai MCP `analyze_video` for understanding video content
- Keep Gemini for transcription with timestamps (audio track)
```

### For aesthetic Skill

1. **Update Workflow 1**:
```markdown
### Workflow 1: Capture & Analyze Inspiration

**Steps**:
1. Browse inspiration sites (Dribbble, Mobbin, Behance, Awwwards)
2. Use **chrome-devtools** skill to capture full-screen screenshots
3. Use **Z.ai MCP Vision tools** to analyze screenshots:
   - Use `analyze_image` for general design analysis
   - Use `extract_text_from_screenshot` for typography extraction
   - Use `ui_diff_check` to compare multiple designs
4. Document findings in project design guidelines
```

2. **Update Workflow 2**:
```markdown
### Workflow 2: Generate & Iterate Design Images

**Steps**:
1. Define design prompt with style, colors, typography
2. Use **image-generation** skill with Nano Banana Pro
3. Use **Z.ai MCP `analyze_image`** to evaluate aesthetic quality
4. If score < 7/10:
   - Identify weaknesses
   - Use **Z.ai MCP `ui_diff_check`** to compare with reference design
   - Refine and regenerate
```

3. **Update Related Skills**:
```markdown
### Related Skills
- **Z.ai MCP Vision**: `analyze_image`, `extract_text_from_screenshot`, `ui_diff_check` for screenshot analysis
- **image-generation**: Generate frontend assets with Nano Banana Pro
- **chrome-devtools**: Capture screenshots from websites
- **ai-multimodal**: Analyze documents, audio, video (keep for non-vision tasks)
```

### For docs-seeker Skill

1. **Update Description**:
```yaml
description: Search internet for technical documentation using Z.ai MCP Search server. Domain filtering, recency filters, and rich summaries for documentation retrieval.
```

2. **Add Tool References**:
```yaml
allowed-tools:
  - mcp__web-search-prime__webSearchPrime
```

3. **Update Workflows**:
```markdown
### Search for Documentation

Use **Z.ai MCP `webSearchPrime`** with domain filtering:

```python
# Search official documentation only
webSearchPrime(
    search_query="Next.js 15 App Router",
    search_domain_filter="nextjs.org",  # Official docs
    search_recency_filter="oneMonth",   # Recent docs
    content_size="high"                  # Detailed summaries
)
```
```

---

## Benefits of Migration

### Performance
- ✅ **Faster OCR**: Specialized model for text extraction
- ✅ **Better Understanding**: Task-specific models (UI, diagrams, errors)
- ✅ **Richer Results**: Web search includes summaries, icons, titles

### Cost
- ✅ **No Gemini Vision Costs**: Offset to Z.ai subscription
- ✅ **Better ROI**: Specialized tools = less iteration needed
- ✅ **Predictable Costs**: Subscription-based pricing

### Features
- ✅ **UI Diff Checking**: Automated comparison (not available in Gemini)
- ✅ **Error Diagnosis**: Specialized debugging (not available in Gemini)
- ✅ **Diagram Understanding**: Better for technical diagrams
- ✅ **Domain Filtering**: Filter search to official docs only

---

## Rollout Plan

### Phase 1: Update Core Skills ✅
- [x] Create migration guide
- [ ] Update ai-multimodal skill
- [ ] Update aesthetic skill
- [ ] Update docs-seeker skill

### Phase 2: Test Integration
- [ ] Test OCR with code screenshots
- [ ] Test UI analysis and diff checking
- [ ] Test web search with domain filters
- [ ] Test diagram understanding

### Phase 3: Documentation
- [ ] Update skill documentation
- [ ] Create example workflows
- [ ] Document best practices

### Phase 4: Monitor & Refine
- [ ] Gather usage feedback
- [ ] Monitor error rates
- [ ] Optimize tool selection
- [ ] Update documentation as needed

---

## Configuration

### Prerequisites

Z.ai MCP servers must be configured in Claude Desktop settings:

```json
{
  "mcpServers": {
    "zai-mcp-server": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@z_ai/mcp-server"],
      "env": {
        "Z_AI_API_KEY": "your_api_key",
        "Z_AI_MODE": "ZAI"
      }
    },
    "web-search-prime": {
      "type": "http",
      "url": "https://api.z.ai/api/mcp/web_search_prime/mcp/",
      "headers": {
        "Authorization": "Bearer your_api_key"
      }
    }
  }
}
```

### Verification

Test MCP tools are available:
```bash
# In Claude Code, run:
"List available MCP tools"

# Expected output should include:
# - mcp__zai-mcp-server__analyze_image
# - mcp__zai-mcp-server__extract_text_from_screenshot
# - mcp__web-search-prime__webSearchPrime
# - etc.
```

---

## Troubleshooting

### Issue: MCP Tools Not Available

**Solution**:
1. Verify MCP server configuration in Claude Desktop settings
2. Restart Claude Code after configuration changes
3. Check API key is valid
4. Test with simple command: `"Analyze this image"` (with image selected)

### Issue: Poor OCR Results

**Solution**:
1. Ensure you're using `extract_text_from_screenshot` (not `analyze_image`)
2. Specify `programming_language` parameter for code screenshots
3. Provide clear prompts: "Extract all Python code from this screenshot"
4. Ensure image is high resolution and not blurry

### Issue: Web Search Not Filtering Domains

**Solution**:
1. Check `search_domain_filter` parameter format (e.g., "nextjs.org", not "https://nextjs.org")
2. Verify domain is accessible and indexed
3. Try without filter first, then add filter

---

## Best Practices

### When to Use Z.ai MCP vs Gemini

**Use Z.ai MCP for**:
- ✅ OCR and text extraction from images
- ✅ UI screenshot analysis and comparison
- ✅ Error diagnosis from screenshots
- ✅ Technical diagram understanding
- ✅ Data visualization analysis
- ✅ Web search with filtering

**Keep Gemini for**:
- ✅ Audio processing and transcription
- ✅ PDF document understanding (no Z.ai equivalent)
- ✅ Long-form video transcription
- ✅ Multimodal reasoning (image + text + audio combined)
- ✅ Image generation (Nano Banana Pro)

### Tool Selection Guide

| Task | Best Tool | Why |
|------|-----------|-----|
| Extract code from screenshot | `extract_text_from_screenshot` | Optimized for code syntax |
| Debug error screenshot | `diagnose_error_screenshot` | Provides actionable fixes |
| Compare designs | `ui_diff_check` | Specialized for UI comparison |
| Convert UI to code | `ui_to_artifact` | Generates actual code |
| Understand architecture diagram | `understand_technical_diagram` | Better for technical diagrams |
| General image description | `analyze_image` | General purpose |
| Search official docs | `webSearchPrime` + domain_filter | Filters to official sources |
| Read web content | `webReader` | Optimized markdown output |

---

## Resources

- [Z.ai Vision MCP Docs](https://docs.z.ai/devpack/mcp/vision-mcp-server)
- [Z.ai Search MCP Docs](https://docs.z.ai/devpack/mcp/search-mcp-server)
- [Z.ai Reader MCP Docs](https://docs.z.ai/devpack/mcp/reader-mcp-server)
- [MCP Protocol Docs](https://modelcontextprotocol.io)
- [Claude Code MCP Guide](https://docs.claude.com/claude-code/mcp)

---

## Summary

**Migration Status**: Ready to implement ✅

**Key Changes**:
1. **ai-multimodal**: Add Z.ai Vision MCP tools for image/OCR tasks
2. **aesthetic**: Use Z.ai Vision for screenshot analysis
3. **docs-seeker**: Use Z.ai Search for web searches
4. Keep Gemini for audio, PDFs, and multimodal tasks

**Expected Benefits**:
- Better OCR accuracy
- Specialized vision tools
- Richer web search results
- Cost savings (subscription vs API usage)
- No API key management for vision tasks

**Next Steps**: Proceed with skill updates following this guide.
