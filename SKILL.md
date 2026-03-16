---
name: xai
version: "2.0.0"
description: "Chat with xAI's Grok models - reasoning, vision analysis, code generation, and multi-turn conversations. Supports Grok-4, Grok-4.20, Grok-3, and Grok-3-mini with function calling. For X/Twitter search, see /search-x."
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
---

# xAI / Grok

Chat with xAI's Grok models. Supports text chat, vision analysis, step-by-step reasoning, code generation, multi-turn conversations, and model comparison.

> **Not for X/Twitter search.** For searching posts on X, use the `/search-x` skill instead. This skill is for direct conversations with Grok - analysis, reasoning, code, and vision.

## Setup

Set your API key:

```bash
# Environment variable (recommended)
export XAI_API_KEY="xai-YOUR-KEY"

# Or via OpenClaw config
openclaw config set skills.entries.xai.apiKey "xai-YOUR-KEY"
```

Get your API key at: https://console.x.ai

## Available Models

| Model | Best for | Context | Notes |
|-------|----------|---------|-------|
| `grok-4` | Complex analysis, long-form reasoning | 131072 | Most capable model |
| `grok-4.20` | Latest capabilities, cutting-edge tasks | 131072 | Newest release |
| `grok-3` | General-purpose tasks, balanced performance | 131072 | Reliable default |
| `grok-3-mini` | Fast responses, step-by-step reasoning | 131072 | Supports reasoning effort |
| `grok-3-fast` | Speed-optimized tasks | 131072 | Lower latency |
| `grok-2-vision-1212` | Image understanding, OCR, diagrams | 32768 | Vision-capable |

### Model selection guide

- **Default for chat:** `grok-3` - good balance of quality and speed
- **Complex problems:** `grok-4` or `grok-4.20` - best reasoning, highest quality
- **Quick answers:** `grok-3-mini` - fast, efficient, good enough for simple tasks
- **Image analysis:** `grok-2-vision-1212` - auto-selected when `--image` is used
- **Reasoning tasks:** `grok-3-mini` with `--reasoning` - exposes chain-of-thought

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

#### Diagram interpretation

```bash
node {baseDir}/scripts/chat.js --image architecture.png "Explain this system architecture diagram. Identify components and data flow."
```

#### Document OCR

```bash
node {baseDir}/scripts/chat.js --image document.jpg "Extract all text from this document. Format as markdown."
```

#### Code screenshot analysis

```bash
node {baseDir}/scripts/chat.js --image code-screenshot.png "What does this code do? Are there any bugs?"
```

#### Supported image formats

- JPEG (.jpg, .jpeg)
- PNG (.png)
- GIF (.gif)
- WebP (.webp)

Images are base64-encoded and sent inline. Keep images under 20MB for reliable processing.

---

### 3. Reasoning Mode

Use `grok-3-mini` for step-by-step reasoning that shows its work. The model exposes its chain-of-thought process, making it ideal for math, logic, and debugging.

```bash
node {baseDir}/scripts/chat.js --model grok-3-mini --system "Think step by step. Show your reasoning before giving the final answer." "If a train leaves Chicago at 8am going 60mph and another leaves New York at 9am going 80mph, when do they meet?"
```

#### Math and logic

```bash
node {baseDir}/scripts/chat.js --model grok-3-mini --system "Show all work step by step." "Prove that the square root of 2 is irrational"
```

#### Debugging reasoning

```bash
node {baseDir}/scripts/chat.js --model grok-3-mini --system "Analyze this problem step by step. Consider edge cases." "Why would a Node.js process leak memory when using closures in event listeners?"
```

#### Decision analysis

```bash
node {baseDir}/scripts/chat.js --model grok-3-mini --system "List pros and cons, then give a recommendation with reasoning." "Should a startup use PostgreSQL or MongoDB for a social media app?"
```

The reasoning mode works by leveraging grok-3-mini's ability to produce structured chain-of-thought output. Use system prompts to control the reasoning style:
- "Think step by step" - sequential logical reasoning
- "Show all work" - math-oriented with intermediate calculations
- "Consider alternatives" - explores multiple approaches
- "Explain your reasoning" - justifies each decision

---

### 4. Code Generation

Grok models are strong at code generation, review, and explanation.

```bash
node {baseDir}/scripts/chat.js --system "You are an expert programmer. Write clean, production-ready code with error handling." "Write a Python function that implements binary search on a sorted array"
```

#### Code review

```bash
node {baseDir}/scripts/chat.js --model grok-4 --system "You are a senior code reviewer. Be thorough but constructive." "Review this function: function debounce(fn, ms) { let t; return (...args) => { clearTimeout(t); t = setTimeout(() => fn(...args), ms); }; }"
```

#### Multi-language translation

```bash
node {baseDir}/scripts/chat.js --system "Convert the following code to Rust. Preserve the logic and add appropriate error handling." "def fibonacci(n): a, b = 0, 1; return [a := b, b := a + b for _ in range(n)]"
```

---

### 5. Multi-Turn Conversations

For multi-turn conversations, chain multiple calls and maintain context by passing previous exchanges in the system prompt.

```bash
# Turn 1: Ask a question
RESPONSE=$(node {baseDir}/scripts/chat.js --json "What are the SOLID principles in software engineering?")

# Turn 2: Follow up with context
node {baseDir}/scripts/chat.js --system "Previous context: The user asked about SOLID principles and you explained them. Now they want to go deeper." "Give me a real-world Python example of the Dependency Inversion Principle"
```

For automated multi-turn workflows, parse the JSON output (`--json`) and feed previous messages back as system context. The xAI API at `api.x.ai/v1/chat/completions` accepts a full messages array - you can build this programmatically:

```bash
# Full messages array approach (for scripts)
curl -s https://api.x.ai/v1/chat/completions \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-3",
    "messages": [
      {"role": "user", "content": "What is recursion?"},
      {"role": "assistant", "content": "Recursion is when a function calls itself..."},
      {"role": "user", "content": "Show me a recursive fibonacci in JavaScript"}
    ]
  }'
```

---

### 6. Model Comparison

Run the same prompt through multiple models to compare quality, speed, and style.

```bash
# Compare grok-3 vs grok-4 on a reasoning task
echo "=== grok-3 ===" && node {baseDir}/scripts/chat.js --model grok-3 "Explain monads in simple terms" && echo -e "\n=== grok-4 ===" && node {baseDir}/scripts/chat.js --model grok-4 "Explain monads in simple terms"
```

#### Quick comparison script

```bash
PROMPT="What are the tradeoffs between REST and GraphQL?"
for model in grok-3 grok-3-mini grok-4; do
  echo "=== $model ==="
  node {baseDir}/scripts/chat.js --model "$model" "$PROMPT"
  echo ""
done
```

This is useful for:
- Evaluating which model handles your use case best
- Checking if grok-3-mini is "good enough" before using grok-4
- Comparing response styles across model generations

---

### 7. List Available Models

Query the xAI API for all models your key has access to.

```bash
node {baseDir}/scripts/models.js
```

---

## Function Calling / Tool Use

The xAI API supports function calling (tool use) via the OpenAI-compatible format. You can define tools in the request body and Grok will return structured function calls.

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

When Grok decides to call a function, the response includes a `tool_calls` array in the assistant message. You then:
1. Execute the function locally
2. Send the result back as a `tool` role message
3. Get Grok's final response incorporating the tool result

This follows the standard OpenAI function calling protocol. All Grok-3 and Grok-4 models support function calling.

---

## Error Recovery

### Common errors and fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `401 Unauthorized` | Invalid or expired API key | Check `XAI_API_KEY`, get a new key at console.x.ai |
| `429 Too Many Requests` | Rate limit exceeded | Wait 60 seconds and retry, or reduce request frequency |
| `404 Not Found` | Model name wrong or unavailable | Run `node {baseDir}/scripts/models.js` to see available models |
| `413 Payload Too Large` | Image too large for vision | Resize image to under 20MB, reduce resolution |
| `500 Internal Server Error` | xAI API issue | Retry after a few seconds, check status.x.ai |
| `ENOTFOUND api.x.ai` | Network issue | Check internet connection |
| Image not found | Bad file path for `--image` | Use absolute path, check file exists |

### Rate limit handling

If you hit rate limits during model comparison or batch operations, add a delay between requests:

```bash
for model in grok-3 grok-3-mini grok-4; do
  echo "=== $model ==="
  node {baseDir}/scripts/chat.js --model "$model" "$PROMPT"
  sleep 2
done
```

### Vision error recovery

If vision analysis fails:
1. Check the image format is supported (JPEG, PNG, GIF, WebP)
2. Verify the file path is correct and the file is readable
3. Ensure the image is under 20MB
4. Try a different image to rule out corruption
5. Fall back to `grok-2-vision-1212` explicitly with `--model grok-2-vision-1212`

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
node {baseDir}/scripts/chat.js --model grok-4 --system "Write production-ready Python code with error handling and type hints." "Write a web scraper using httpx and BeautifulSoup that extracts article titles and URLs from a news site"
```

**User:** "Quick question for Grok"
**Action:**
```bash
node {baseDir}/scripts/chat.js --model grok-3-mini "What's the time complexity of quicksort?"
```

### Vision examples

**User:** "Grok, analyze this screenshot"
**Action:**
```bash
node {baseDir}/scripts/chat.js --image /path/to/screenshot.png "Analyze this UI screenshot. Identify usability issues, accessibility problems, and suggest improvements."
```

**User:** "OCR this document"
**Action:**
```bash
node {baseDir}/scripts/chat.js --image /path/to/document.jpg "Extract all text from this document. Preserve formatting and structure. Output as markdown."
```

**User:** "What's in this diagram?"
**Action:**
```bash
node {baseDir}/scripts/chat.js --image /path/to/diagram.png "Explain this diagram in detail. Identify all components, relationships, and data flows."
```

### Reasoning examples

**User:** "Walk me through this math problem"
**Action:**
```bash
node {baseDir}/scripts/chat.js --model grok-3-mini --system "Show all work step by step." "Solve: integral of x^2 * e^x dx"
```

**User:** "Debug this logic"
**Action:**
```bash
node {baseDir}/scripts/chat.js --model grok-3-mini --system "Think step by step about what could go wrong." "Why would this SQL query return duplicates: SELECT u.name, o.total FROM users u JOIN orders o ON u.id = o.user_id WHERE o.status = 'completed'"
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

- **/search-x** - Search X/Twitter for real-time posts, trends, and discussions. Uses xAI's Responses API with the x_search tool. Use this when you need to find what people are saying on X.
- **/last30days** - Research what's happened in the last 30 days across the web. Combines multiple sources for comprehensive trend analysis.

---

## API Reference

- xAI API Docs: https://docs.x.ai/api
- xAI Console (API keys): https://console.x.ai
- API Base URL: `https://api.x.ai`
- Chat Completions: `POST /v1/chat/completions` (OpenAI-compatible)
- Models: `GET /v1/models`

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `XAI_API_KEY` | Yes | - | Your xAI API key from console.x.ai |
| `XAI_MODEL` | No | `grok-3` | Default model for chat requests |
