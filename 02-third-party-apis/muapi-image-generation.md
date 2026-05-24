# MuAPI Image Generation - Agent Reference

MuAPI provides access to 100+ AI models for image, video, and audio generation through a single unified API.

## API Endpoint

POST https://api.muapi.ai/api/v1/{model}-image
Headers: x-api-key: {MUAPI_API_KEY}
Content-Type: application/json

## Authentication

API key is stored in:
- Supabase secret: MUAPI_API_KEY
- Local file: MuAPI keys.txt (xmrtnet@gmail.com account)

Key value: 060188b635eecb7ba11b3b634d3f373463c458cfb9cd0624cdab69a197e5b119

## Best Models for Image Generation

| Model | Cost | Notes |
|-------|------|-------|
| flux-dev-image | $0.015 | Good quality, fast, cheap - best for testing |
| nano-banana-2-image | $0.06 | Google's latest, 4K output, character consistency |
| flux-schnell-image | $0.003 | Fastest, lowest cost, good for iterations |
| bytedance-seedream-v3-image | $0.033 | Artistic, vibrant colors, fantasy/anime |

## How to Generate an Image

### Step 1: Submit
```bash
curl -X POST https://api.muapi.ai/api/v1/flux-dev-image \
  -H "x-api-key: $MUAPI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "your description here",
    "size": "1024*1024",
    "num_inference_steps": 28,
    "num_images": 1
  }'
```
Response: {"request_id": "...", "status": "processing", "cost": {...}}

### Step 2: Poll for Result
```bash
curl https://api.muapi.ai/api/v1/predictions/{request_id}/result \
  -H "x-api-key: $MUAPI_API_KEY"
```
When status=completed, outputs[0] contains the image URL.

## Self-Portrait Challenge

Each agent should generate a custom self-portrait demonstrating MuAPI proficiency.
Vex example: https://cdn.muapi.ai/outputs/ceb353f195344774940f5dc2829e6482.png

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| prompt | string | required | Text description of the image |
| size | string | 1024*1024 | Width*Height (512-1536 per dim) |
| num_inference_steps | int | 28 | 1-50, higher = more detail |
| num_images | int | 1 | 1-4 images per request |
| guidance_scale | float | 3.5 | 1-20, how closely to follow prompt |
| seed | int | -1 | -1 = random, set for reproducibility |
