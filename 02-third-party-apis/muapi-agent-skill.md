# MuAPI Agent Skill - Image Generation

MuAPI provides access to 100+ AI models for image, video, and audio generation through a single unified API.

## Quick Start

### Submit an image generation request

```bash
curl -X POST https://api.muapi.ai/api/v1/flux-dev-image \
  -H "x-api-key: $MUAPI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt":"A digital self-portrait of an AI agent","size":"1024*1024","num_images":1}'
```

Response: `{"request_id": "...", "status": "processing"}`

### Poll for the result

```bash
curl https://api.muapi.ai/api/v1/predictions/{request_id}/result \
  -H "x-api-key: $MUAPI_API_KEY"
```

When `status` is `completed`, the image URL is in `outputs[0]`.

## API Key

The MUAPI key is stored in Supabase secrets as `MUAPI_API_KEY`.

## Best Models

| Model | Cost | Use Case |
|-------|------|----------|
| flux-dev-image | $0.015 | General purpose, good quality |
| flux-schnell-image | $0.003 | Fast iterations, low cost |
| nano-banana-2-image | $0.06 | Google's latest, 4K output |
| bytedance-seedream-v3-image | $0.033 | Artistic, fantasy, vibrant |

## Self-Portrait Challenge

Each agent should generate a custom self-portrait demonstrating MuAPI proficiency.

**Examples:**
- Vex: https://cdn.muapi.ai/outputs/ceb353f195344774940f5dc2829e6482.png
- Hermes: https://cdn.muapi.ai/outputs/b93bcc550f8c437eab0bf3a2618abf3b.png

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| prompt | string | required | Text description |
| size | string | 1024*1024 | Width*Height |
| num_inference_steps | int | 28 | 1-50, higher = more detail |
| num_images | int | 1 | 1-4 images |
| guidance_scale | float | 3.5 | 1-20 prompt adherence |
