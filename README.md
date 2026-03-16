# xAI / Grok Skill for OpenClaw

Chat with xAI's [Grok](https://x.ai) models. Text, vision analysis, reasoning, code generation, and multi-turn conversations.

## What it does

- **Chat with Grok** - text conversations with Grok-4, Grok-4.20, Grok-3, Grok-3-mini, Grok-3-fast
- **Vision analysis** - analyze images, screenshots, diagrams, OCR documents
- **Reasoning mode** - step-by-step chain-of-thought with Grok-3-mini
- **Code generation** - write, review, and translate code across languages
- **Model comparison** - run the same prompt through multiple models
- **Function calling** - structured tool use via OpenAI-compatible format
- **Multi-turn conversations** - maintain context across exchanges

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
- "Walk me through this math problem step by step"
- "Have Grok write a Python web scraper"
- "Compare Grok-3 and Grok-4 on this question"
- "What Grok models are available?"

## Available models

| Model | Best for |
|-------|----------|
| `grok-4` | Complex analysis, long-form reasoning |
| `grok-4.20` | Latest capabilities, cutting-edge tasks |
| `grok-3` | General-purpose, balanced (default) |
| `grok-3-mini` | Fast responses, step-by-step reasoning |
| `grok-3-fast` | Speed-optimized |
| `grok-2-vision-1212` | Image understanding, OCR |

## CLI usage

```bash
# Chat
node scripts/chat.js "What is quantum computing?"
node scripts/chat.js --model grok-4 "Explain monads"
node scripts/chat.js --system "You are a poet" "Write about the moon"

# Vision
node scripts/chat.js --image screenshot.png "What UI issues do you see?"
node scripts/chat.js --image document.jpg "Extract all text as markdown"

# Reasoning
node scripts/chat.js --model grok-3-mini --system "Think step by step." "Prove sqrt(2) is irrational"

# Streaming
node scripts/chat.js --stream "Write a short story"

# List models
node scripts/models.js
```

## How it works

Wraps the xAI API (`https://api.x.ai`). Chat uses the `/v1/chat/completions` endpoint (OpenAI-compatible). Vision uses the same endpoint with base64-encoded images. Function calling follows the standard OpenAI tool use protocol.

- API docs: https://docs.x.ai/api

## License

MIT
