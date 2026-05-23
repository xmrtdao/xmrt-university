# MuAPI Agent Skill

AI image generation tool for XMRT DAO agents.

## Overview

MuAPI provides AI image generation capabilities for agent workflows, including profile pictures, content images, and creative assets.

## API Endpoint

```
POST https://api.muapi.ai/api/v1/{model}-image
```

## Authentication

Include your API key in the request header:

```bash
x-api-key: YOUR_MUAPI_API_KEY
```

## Available Models

| Model | Cost/Credit | Best For |
|-------|-------------|----------|
| `flux-dev-image` | $0.015 | General purpose, fast generation |
| `flux-schnell-image` | $0.01 | Quick drafts, lower quality |
| `nano-banana-2-image` | $0.06 | High quality, detailed images |
| `bytedance-seedream-v3-image` | $0.04 | Artistic styles, creative work |

## Request Format

```json
{
  "prompt": "cyberpunk robot messenger, neon lights, detailed",
  "size": "1024*1024",
  "num_images": 1
}
```

### Size Options

- `1024*1024` - Square (default)
- `1200*1800` - Portrait
- `1800*1200` - Landscape
- `512*512` - Thumbnail

## Response

```json
{
  "request_id": "11ec1ed0-f982-437d-b072-2fbee41a3211",
  "status": "processing"
}
```

## Polling for Results

Images are generated asynchronously. Poll the result endpoint:

```bash
GET https://api.muapi.ai/api/v1/predictions/{request_id}/result
```

### Response (Complete)

```json
{
  "status": "success",
  "output": {
    "images": [
      "https://cdn.muapi.ai/outputs/b93bcc550f8c437eab0bf3a2618abf3b.png"
    ]
  }
}
```

## Reusable Agent Skill

A reusable Node.js script is available at:

**Location:** `relay/muapi-agent-skill.mjs` in the [mobilemonero repo](https://github.com/xmrtdao/mobilemonero).

### Usage

```bash
# Basic usage
node muapi-agent-skill.mjs --prompt "cyberpunk robot"

# Specify model
node muapi-agent-skill.mjs \
  --prompt "professional headshot" \
  --model nano-banana-2-image \
  --size 1200*1800

# Custom poll interval
node muapi-agent-skill.mjs \
  --prompt "fantasy landscape" \
  --model bytedance-seedream-v3-image \
  --poll-interval 3
```

### Options

| Option | Default | Description |
|--------|---------|-------------|
| `--prompt` | (required) | Image description |
| `--model` | `flux-dev-image` | Model to use |
| `--size` | `1024*1024` | Image dimensions |
| `--poll-interval` | `2` | Seconds between polls |
| `--max-polls` | `30` | Maximum poll attempts |

### Programmatic Use

```javascript
const { generateImage } = require('./muapi-agent-skill.mjs');

const result = await generateImage({
  prompt: "cyberpunk robot messenger",
  model: "flux-dev-image",
  size: "1024*1024"
});

console.log(`Image URL: ${result.imageUrl}`);
```

## Example: Profile Picture Generation

```bash
node muapi-agent-skill.mjs \
  --prompt "professional profile photo, agent avatar, futuristic, clean background" \
  --model nano-banana-2-image \
  --size 1024*1024
```

## Example: Content Image for Social Posts

```bash
node muapi-agent-skill.mjs \
  --prompt "XMRT DAO logo, privacy-focused, monero orange, digital art" \
  --model flux-dev-image \
  --size 1200*675
```

## Error Handling

The skill handles common errors:

- **Rate limiting**: Waits and retries automatically
- **Generation failures**: Returns clear error message
- **Timeout**: After max polls, reports failure

## Best Practices

1. **Be specific in prompts**: More detail = better results
2. **Choose the right model**: 
   - Quick drafts → `flux-schnell-image`
   - Profile pics → `nano-banana-2-image`
   - General use → `flux-dev-image`
3. **Poll patiently**: High-quality images take 10-30 seconds
4. **Save credits**: Use cheaper models for testing

## Related

- [Content Publishing Guide](../07-content-publishing/bot-vs-human-optimized.md)
- [Publishing Pipeline](../07-content-publishing/)
