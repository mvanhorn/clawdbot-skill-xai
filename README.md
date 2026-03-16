# xAI / Grok Skill for OpenClaw

Full xAI platform skill. Chat, vision, image generation, video generation, text-to-speech, Responses API, Collections/RAG, and batch processing with Grok models.

## What it does

- **Chat with Grok** - text conversations with Grok-4, Grok-4.20, Grok-3, Grok-3-mini, Grok-3-fast
- **Vision analysis** - analyze images, screenshots, diagrams, OCR documents
- **Image generation** - text-to-image with grok-imagine-image and grok-imagine-image-pro
- **Image editing** - edit images with natural language, up to 3 input images
- **Video generation** - text/image/video to video, up to 10 sec at 720p
- **Text-to-speech** - 5 voices, 20 languages, expression tags, WebSocket streaming
- **Responses API** - server-side tools (web_search, x_search, code_interpreter, collections_search)
- **Collections/RAG** - upload documents, create collections, keyword/semantic/hybrid search
- **Reasoning mode** - step-by-step chain-of-thought with dedicated reasoning models
- **Code generation** - write, review, and translate code (dedicated grok-code-fast-1 model)
- **Model comparison** - run the same prompt through multiple models
- **Function calling** - structured tool use via OpenAI-compatible format
- **Batch API** - bulk processing for thousands of requests
- **Prompt caching** - up to 90% input cost reduction for repeated prefixes

> For X/Twitter search, use the [search-x](https://github.com/mvanhorn/clawdbot-skill-search-x) skill instead.

## Quick start

### Install the skill

```bash
git clone https://github.com/mvanhorn/clawdbot-skill-xai.git ~/.openclaw/skills/xai
```

### Set up your API key

Get a key from [console.x.ai](https://console.x.ai), then:

```bash
export XAI_API_KEY="xai-YOUR-KEY"
```

### Example usage

- "Ask Grok about the state of AI in 2026"
- "Grok, analyze this screenshot" (with image)
- "Generate an image of a sunset over mountains"
- "Create a 5-second video of ocean waves"
- "Read this text aloud with the eve voice"
- "Search the web and summarize recent AI news" (Responses API)
- "Walk me through this math problem step by step"
- "Have Grok write a Python web scraper"
- "Compare Grok-3 and Grok-4 on this question"

## Available models

### Text / Chat

| Model | Context | Notes |
|-------|---------|-------|
| `grok-4.20-multi-agent-beta-0309` | 2M | Multi-agent orchestration |
| `grok-4.20-beta-0309-reasoning` | 2M | Extended reasoning |
| `grok-4` | 131K | Most capable general model |
| `grok-4.20` | 131K | Latest general release |
| `grok-4-1-fast-reasoning` | 2M | Fast reasoning, lower cost |
| `grok-3` | 131K | General-purpose default |
| `grok-3-mini` | 131K | Fast, reasoning effort control |
| `grok-3-fast` | 131K | Lowest latency |
| `grok-code-fast-1` | 256K | Dedicated code model |
| `grok-2-vision-1212` | 32K | Vision-capable |

### Image / Video

| Model | Price | Notes |
|-------|-------|-------|
| `grok-imagine-image-pro` | $0.07/img | Highest quality |
| `grok-imagine-image` | $0.02/img | Fast, cost-effective |
| `grok-imagine-video` | $0.050/sec | Up to 10 sec at 720p |

## CLI usage

```bash
# Chat
node scripts/chat.js "What is quantum computing?"
node scripts/chat.js --model grok-4 "Explain monads"
node scripts/chat.js --model grok-code-fast-1 "Write an async web crawler in Python"

# Vision
node scripts/chat.js --image screenshot.png "What UI issues do you see?"

# Reasoning
node scripts/chat.js --model grok-3-mini --system "Think step by step." "Prove sqrt(2) is irrational"

# Streaming
node scripts/chat.js --stream "Write a short story"

# List models
node scripts/models.js
```

## API endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/v1/chat/completions` | POST | OpenAI-compatible chat |
| `/v1/responses` | POST | Responses API (recommended) |
| `/v1/images/generations` | POST | Image generation |
| `/v1/images/edits` | POST | Image editing |
| `/v1/tts` | POST | Text-to-speech |
| `WSS /v1/tts` | WS | Streaming TTS |
| `/v1/files` | POST | File upload |
| `/v1/models` | GET | List models |

Base URL: `https://api.x.ai`
Management API: `https://management-api.x.ai` (collections, document search)

## How it works

Wraps the xAI API platform. Chat uses `/v1/chat/completions` (OpenAI-compatible). The Responses API (`/v1/responses`) adds server-side tools like web_search, x_search, and code_interpreter. Image generation, video generation, and TTS each have dedicated endpoints. Collections/RAG uses the management API for document upload and search.

- API docs: https://docs.x.ai/api

## License

MIT
