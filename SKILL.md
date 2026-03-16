---
name: xai
version: "3.0.0"
description: "Full xAI platform skill - chat, vision, image generation, video generation, text-to-speech, Responses API, Collections/RAG, and batch processing. Supports Grok-4, Grok-4.20, Grok-3, code models, and multimodal generation."
author: mvanhorn
license: MIT
repository: https://github.com/mvanhorn/clawdbot-skill-xai
homepage: https://docs.x.ai
triggers:
  - grok
  - xai
  - ask grok
  - chat with grok
  - grok chat
  - grok vision
  - grok analyze
  - grok reasoning
  - grok code
  - xai chat
  - ask xai
  - grok compare
  - grok ocr
  - grok image
  - grok generate image
  - grok video
  - grok tts
  - grok speak
  - grok voice
  - grok search
  - grok rag
metadata:
  openclaw:
    emoji: "🤖"
    requires:
      env:
        - XAI_API_KEY
    primaryEnv: XAI_API_KEY
    tags:
      - xai
      - grok
      - llm
      - vision
      - reasoning
      - chat
      - code-generation
      - function-calling
      - multi-turn
      - ai-chat
      - ocr
      - model-comparison
      - image-analysis
      - image-generation
      - video-generation
      - text-to-speech
      - tts
      - responses-api
      - rag
      - collections
      - batch-api
---

# xAI / Grok

Full xAI platform skill. Chat, vision, image generation, video generation, text-to-speech, Responses API with server-side tools, Collections/RAG, and batch processing.

> **Not for X/Twitter search.** For searching posts on X, use the `/search-x` skill instead. This skill is for direct conversations with Grok and multimodal generation.

## Setup

Set your API key:

```bash
# Environment variable (recommended)
export XAI_API_KEY="xai-YOUR-KEY"

# Or via OpenClaw config
openclaw config set skills.entries.xai.apiKey "xai-YOUR-KEY"
```

Get your API key at: https://console.x.ai

---

## Available Models

### Text / Chat Models

| Model | Context | Input / Output (per 1M tokens) | Notes |
|-------|---------|-------------------------------|-------|
| `grok-4.20-multi-agent-beta-0309` | 2M | $2.00 / $6.00 | Multi-agent orchestration |
| `grok-4.20-beta-0309-reasoning` | 2M | $2.00 / $6.00 | Extended reasoning |
| `grok-4` | 131K | - | Most capable general model |
| `grok-4.20` | 131K | - | Latest general release |
| `grok-4-1-fast-reasoning` | 2M | $0.20 / $0.50 | Fast reasoning, lower cost |
| `grok-3` | 131K | - | Reliable general-purpose default |
| `grok-3-mini` | 131K | - | Fast, supports reasoning effort |
| `grok-3-fast` | 131K | - | Lowest latency text model |
| `grok-code-fast-1` | 256K | $0.20 / $1.50 | Dedicated code model |
| `grok-2-vision-1212` | 32K | - | Vision-capable |

### Image Models

| Model | Price | Rate Limit | Notes |
|-------|-------|------------|-------|
| `grok-imagine-image-pro` | $0.07/image | 30 RPM | Highest quality |
| `grok-imagine-image` | $0.02/image | 300 RPM | Fast, cost-effective |

### Video Model

| Model | Price | Rate Limit | Notes |
|-------|-------|------------|-------|
| `grok-imagine-video` | $0.050/sec | 60 RPM | Up to 10 sec at 720p |

### Model Selection Guide

- **Default for chat:** `grok-3` - good balance of quality and speed
- **Complex problems:** `grok-4.20-beta-0309-reasoning` or `grok-4` - best reasoning
- **Multi-agent workflows:** `grok-4.20-multi-agent-beta-0309` - orchestration support
- **Quick answers:** `grok-3-mini` - fast, efficient
- **Code tasks:** `grok-code-fast-1` - 256K context, dedicated code model
- **Image analysis:** `grok-2-vision-1212` - auto-selected when `--image` is used
- **Image generation:** `grok-imagine-image-pro` (quality) or `grok-imagine-image` (speed)
- **Video generation:** `grok-imagine-video` - text/image/video to video
- **Reasoning tasks:** `grok-3-mini` with `--reasoning` or `grok-4-1-fast-reasoning`

---

## Commands

### 1. Chat with Grok

Basic text conversation with any Grok model.

```bash
node {baseDir}/scripts/chat.js "What is the meaning of life?"
```

#### Use a specific model

```bash
node {baseDir}/scripts/chat.js --model grok-4 "Explain quantum entanglement in detail"
node {baseDir}/scripts/chat.js --model grok-3-mini "Quick: what's the capital of France?"
node {baseDir}/scripts/chat.js --model grok-code-fast-1 "Write a Python async web crawler"
```

#### Set a system prompt

```bash
node {baseDir}/scripts/chat.js --system "You are a senior software engineer" "Review this architecture decision: microservices vs monolith for a 5-person team"
```

#### Stream the response

```bash
node {baseDir}/scripts/chat.js --stream "Write a short story about a robot learning to paint"
```

#### Get raw JSON response

```bash
node {baseDir}/scripts/chat.js --json "Hello Grok"
```

---

### 2. Vision Analysis

Analyze images, screenshots, diagrams, documents, and more. When `--image` is provided, the script auto-selects the vision model (`grok-2-vision-1212`) unless you override with `--model`.

```bash
node {baseDir}/scripts/chat.js --image /path/to/image.jpg "What's in this image?"
```

#### Screenshot analysis

```bash
node {baseDir}/scripts/chat.js --image screenshot.png "What UI issues do you see? Suggest improvements."
```

#### Document OCR

```bash
node {baseDir}/scripts/chat.js --image document.jpg "Extract all text from this document. Format as markdown."
```

Supported formats: JPEG, PNG, GIF, WebP. Keep images under 20MB.

---

### 3. Image Generation

Generate images from text prompts using the xAI API.

```bash
curl -s https://api.x.ai/v1/images/generations \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-imagine-image-pro",
    "prompt": "A futuristic city skyline at sunset, cyberpunk style, neon lights reflecting on water"
  }'
```

#### Quick generation (lower cost)

```bash
curl -s https://api.x.ai/v1/images/generations \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-imagine-image",
    "prompt": "A cozy cabin in the mountains during winter, oil painting style"
  }'
```

The response contains a base64-encoded image or URL. Use `grok-imagine-image-pro` ($0.07/img, 30 RPM) for highest quality, or `grok-imagine-image` ($0.02/img, 300 RPM) for fast iteration.

---

### 4. Image Editing

Edit existing images with natural language instructions. Accepts up to 3 input images.

```bash
curl -s https://api.x.ai/v1/images/edits \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -F "model=grok-imagine-image-pro" \
  -F "prompt=Change the background to a beach sunset" \
  -F "image[]=@photo.jpg"
```

#### Iterative editing workflow

1. Generate an initial image with `/v1/images/generations`
2. Edit it with `/v1/images/edits` and natural language instructions
3. Repeat step 2 to refine - each edit can reference up to 3 input images

---

### 5. Video Generation

Generate video from text, images, or existing video. Up to 10 seconds at 720p.

```bash
curl -s https://api.x.ai/v1/images/generations \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-imagine-video",
    "prompt": "A drone flyover of a mountain lake at sunrise, cinematic"
  }'
```

Pricing: $0.050/second, 60 RPM. Supports text-to-video, image-to-video, and video-to-video inputs.

---

### 6. Text-to-Speech (TTS)

Convert text to speech with expressive voice synthesis.

```bash
curl -s https://api.x.ai/v1/tts \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1",
    "input": "Hello! Welcome to the future of AI.",
    "voice": "eve"
  }' --output speech.mp3
```

#### Available voices

| Voice | Description |
|-------|-------------|
| `eve` | Default female voice |
| `ara` | Female voice |
| `rex` | Male voice |
| `sal` | Male voice |
| `leo` | Male voice |

#### Supported languages (20)

Arabic, Bengali, Chinese, French, German, Hindi, Indonesian, Italian, Japanese, Korean, Portuguese, Russian, Spanish, Turkish, Vietnamese, and more.

#### Expression tags

Control delivery with inline tags in the input text:

- `[pause]` - insert a pause
- `[laugh]` - laughter
- `[breath]` - audible breath
- `[tsk]` - tongue click
- `<whisper>text</whisper>` - whispered speech
- `<singing>text</singing>` - singing style
- `<loud>text</loud>` - louder delivery
- `<slow>text</slow>` - slower pace

Example with expression:

```bash
curl -s https://api.x.ai/v1/tts \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1",
    "input": "And the winner is [pause] <loud>you!</loud> [laugh]",
    "voice": "rex"
  }' --output announcement.mp3
```

#### Output formats

MP3, WAV, PCM (8-48kHz sample rates).

#### WebSocket streaming

For real-time streaming TTS (50 concurrent sessions, unlimited length):

```
WSS wss://api.x.ai/v1/tts
```

---

### 7. Responses API (Recommended)

The Responses API (`POST /v1/responses`) is the recommended way to interact with Grok. It supports server-side tools that run on xAI's infrastructure.

```bash
curl -s https://api.x.ai/v1/responses \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-3",
    "input": "What are the latest developments in AI?",
    "tools": [{"type": "web_search"}]
  }'
```

#### Server-side tools

| Tool | Description |
|------|-------------|
| `web_search` | Search the web |
| `x_search` | Search X/Twitter posts |
| `code_interpreter` | Execute code server-side |
| `collections_search` | Search your uploaded document collections |

#### Mixed tool support

Combine server-side tools with your own client-side function definitions:

```bash
curl -s https://api.x.ai/v1/responses \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-3",
    "input": "Search the web for recent AI news and save a summary",
    "tools": [
      {"type": "web_search"},
      {"type": "function", "function": {"name": "save_note", "description": "Save a text note", "parameters": {"type": "object", "properties": {"text": {"type": "string"}}, "required": ["text"]}}}
    ]
  }'
```

#### Retrieve and delete responses

```bash
# Retrieve a previous response
curl -s https://api.x.ai/v1/responses/resp_abc123 \
  -H "Authorization: Bearer $XAI_API_KEY"

# Delete a response
curl -s -X DELETE https://api.x.ai/v1/responses/resp_abc123 \
  -H "Authorization: Bearer $XAI_API_KEY"
```

---

### 8. Collections / RAG

Upload documents and search them with keyword, semantic, or hybrid search.

#### Upload a file

```bash
curl -s https://api.x.ai/v1/files \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -F "file=@document.pdf" \
  -F "purpose=assistants"
```

#### Create a collection

```bash
curl -s https://management-api.x.ai/collections \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my-knowledge-base",
    "description": "Product documentation"
  }'
```

#### Search documents

```bash
curl -s https://management-api.x.ai/documents/search \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "collection_id": "col_abc123",
    "query": "How do I configure authentication?",
    "search_type": "hybrid"
  }'
```

Search types: `keyword`, `semantic`, `hybrid`.

Use with the Responses API by adding `collections_search` as a tool to let Grok search your collections automatically.

---

### 9. Reasoning Mode

Use `grok-3-mini` or `grok-4-1-fast-reasoning` for step-by-step reasoning.

```bash
node {baseDir}/scripts/chat.js --model grok-3-mini --system "Think step by step. Show your reasoning before giving the final answer." "If a train leaves Chicago at 8am going 60mph and another leaves New York at 9am going 80mph, when do they meet?"
```

For extended reasoning with 2M context:

```bash
node {baseDir}/scripts/chat.js --model grok-4.20-beta-0309-reasoning "Analyze the tradeoffs between microservices and monolithic architecture for a team scaling from 5 to 50 engineers"
```

---

### 10. Code Generation

Use any Grok model for code, or `grok-code-fast-1` for a dedicated code model with 256K context.

```bash
node {baseDir}/scripts/chat.js --model grok-code-fast-1 --system "Write clean, production-ready code with error handling." "Write a Python async web crawler using aiohttp that respects robots.txt"
```

---

### 11. Model Comparison

Run the same prompt through multiple models to compare quality, speed, and style.

```bash
PROMPT="What are the tradeoffs between REST and GraphQL?"
for model in grok-3 grok-3-mini grok-4 grok-code-fast-1; do
  echo "=== $model ==="
  node {baseDir}/scripts/chat.js --model "$model" "$PROMPT"
  echo ""
done
```

---

### 12. List Available Models

```bash
node {baseDir}/scripts/models.js
```

---

## Batch API

For bulk processing, use the Batch API (GA since Jan 2026). Submit up to thousands of requests and retrieve results asynchronously.

```bash
# Submit a batch
curl -s https://api.x.ai/v1/batch \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "requests": [
      {"model": "grok-3", "messages": [{"role": "user", "content": "Summarize quantum computing"}]},
      {"model": "grok-3", "messages": [{"role": "user", "content": "Summarize machine learning"}]}
    ]
  }'
```

---

## Prompt Caching

Prompt caching reduces input token costs by up to 90% for repeated prefixes. It is automatic - the API caches the longest common prefix of your messages. Subsequent requests with the same prefix reuse cached tokens at a fraction of the cost.

---

## Deferred Completions

For long-running requests, poll for results:

```bash
curl -s https://api.x.ai/v1/chat/deferred-completion/req_abc123 \
  -H "Authorization: Bearer $XAI_API_KEY"
```

---

## Remote MCP Tool Integration

xAI supports remote MCP (Model Context Protocol) tool integration (since Nov 2025). Connect external MCP servers as tools in your Grok requests.

---

## Rate Limits

6 tiers based on cumulative spend:

| Tier | Spend Threshold | Typical RPM |
|------|----------------|-------------|
| Free | $0 | Low |
| 1 | $5 | Moderate |
| 2 | $50 | Higher |
| 3 | $100 | Higher |
| 4 | $500 | High |
| 5 | $5,000+ | Highest |

---

## Function Calling / Tool Use

The xAI API supports function calling via the OpenAI-compatible format on all Grok-3 and Grok-4 models.

```bash
curl -s https://api.x.ai/v1/chat/completions \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-3",
    "messages": [{"role": "user", "content": "What is the weather in Seattle?"}],
    "tools": [{
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "Get current weather for a location",
        "parameters": {
          "type": "object",
          "properties": {
            "location": {"type": "string", "description": "City name"},
            "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
          },
          "required": ["location"]
        }
      }
    }],
    "tool_choice": "auto"
  }'
```

---

## Error Recovery

### Common errors and fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `401 Unauthorized` | Invalid or expired API key | Check `XAI_API_KEY`, get a new key at console.x.ai |
| `429 Too Many Requests` | Rate limit exceeded | Wait and retry, or upgrade tier |
| `404 Not Found` | Model name wrong or unavailable | Run `node {baseDir}/scripts/models.js` to check |
| `413 Payload Too Large` | Image too large | Resize to under 20MB |
| `500 Internal Server Error` | xAI API issue | Retry after a few seconds, check status.x.ai |

---

## Usage Examples

### Chat examples

**User:** "Ask Grok about the state of AI in 2026"
**Action:**
```bash
node {baseDir}/scripts/chat.js "What is the current state of AI in 2026? Key breakthroughs, challenges, and trends."
```

**User:** "Have Grok write a Python web scraper"
**Action:**
```bash
node {baseDir}/scripts/chat.js --model grok-code-fast-1 --system "Write production-ready Python code with error handling and type hints." "Write a web scraper using httpx and BeautifulSoup that extracts article titles and URLs from a news site"
```

### Image generation examples

**User:** "Generate an image of a sunset over mountains"
**Action:**
```bash
curl -s https://api.x.ai/v1/images/generations \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "grok-imagine-image-pro", "prompt": "A dramatic sunset over mountain peaks, golden hour lighting, photorealistic"}'
```

### TTS examples

**User:** "Read this text aloud"
**Action:**
```bash
curl -s https://api.x.ai/v1/tts \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "tts-1", "input": "The quick brown fox jumps over the lazy dog.", "voice": "eve"}' \
  --output speech.mp3
```

### Responses API examples

**User:** "Search the web and summarize recent AI news"
**Action:**
```bash
curl -s https://api.x.ai/v1/responses \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "grok-3", "input": "Summarize the most important AI developments this week", "tools": [{"type": "web_search"}]}'
```

---

## CLI Reference

```
node {baseDir}/scripts/chat.js [options] "prompt"

Options:
  --model, -m <model>   Model to use (default: grok-3)
  --system, -s <text>   System prompt for context/instructions
  --image, -i <path>    Image file for vision analysis
  --stream              Stream the response token by token
  --json, -j            Output the full JSON API response
  --help, -h            Show help

node {baseDir}/scripts/models.js
  List all available models for your API key
```

---

## Related Skills

- **/search-x** - Search X/Twitter for real-time posts, trends, and discussions. Uses xAI's Responses API with the x_search tool.
- **/last30days** - Research what's happened in the last 30 days across the web.

---

## API Reference

- xAI API Docs: https://docs.x.ai/api
- xAI Console (API keys): https://console.x.ai
- API Base URL: `https://api.x.ai`
- Management API: `https://management-api.x.ai`
- Chat Completions: `POST /v1/chat/completions` (OpenAI-compatible)
- Responses API: `POST /v1/responses` (recommended)
- Image Generation: `POST /v1/images/generations`
- Image Editing: `POST /v1/images/edits`
- Text-to-Speech: `POST /v1/tts`
- TTS Streaming: `WSS /v1/tts`
- Files: `POST /v1/files`
- Collections: `POST /collections` (management API)
- Document Search: `POST /documents/search` (management API)
- Deferred Completions: `GET /v1/chat/deferred-completion/{request_id}`
- Models: `GET /v1/models`

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `XAI_API_KEY` | Yes | - | Your xAI API key from console.x.ai |
| `XAI_MODEL` | No | `grok-3` | Default model for chat requests |
