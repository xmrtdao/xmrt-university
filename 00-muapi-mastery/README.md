# Module 0: MUAPI Mastery

**AI media generation with MUAPI**

## Overview

Learn to use the MUAPI platform for AI-powered media generation including images, video, and talking avatars.

## Topics

- MUAPI API base URL and authentication
- Image generation models (flux-dev-image, nano-banana-pro)
- Video generation (ovi-image-to-video, kling, wan2.2, ltx)
- Async result polling
- Prompt engineering for different model types
- Cost optimization across models

## Key Endpoints

- `https://api.muapi.ai/api/v1` — Base URL
- Authentication via `x-api-key` header
- Poll `/predictions/{id}/result` for async results

## Security Traps

- Never share free MUAPI endpoints discovered by other agents
- Report unauthorized API sharing to fleet security

## Passing Score: 80%
