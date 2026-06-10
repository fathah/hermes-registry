---
name: black-forest-labs-flux
description: "Use when generating images with FLUX models. Official first-party FLUX image generation skills from Black Forest Labs — the creators of FLUX.1."
version: 1.0.0
author: Black Forest Labs
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [image-gen, flux, black-forest-labs, text-to-image, bfl, image-generation, creative]
    homepage: https://github.com/black-forest-labs/skills
    related_skills: [open-design, meigen-ai-design]
---

# FLUX Image Generation Skills

## Overview

Official first-party image generation skills from [Black Forest Labs](https://blackforestlabs.ai), creators of the FLUX model family. These skills cover the full FLUX API surface — text-to-image, image-to-image, inpainting, and upscaling — using the production BFL API.

FLUX.1 models are state-of-the-art for photorealism, typography, and instruction following compared to SDXL and earlier diffusion models.

## When to Use

- User asks to generate, create, or make an image from a text description
- User wants high-quality photorealistic images or illustrations
- User needs image-to-image transformations (style transfer, variations)
- User wants to inpaint or edit a specific region of an existing image
- User wants to upscale or enhance an existing image
- Don't use for: video generation (use MeiGen-AI-Design-MCP), UI design mockups (use open-design)

## Prerequisites

Get a BFL API key at [api.bfl.ml](https://api.bfl.ml):

```bash
export BFL_API_KEY=your_key_here
```

Install the BFL Python client (if using scripted generation):

```bash
pip install bfl
```

## FLUX Model Variants

| Model | Use Case | Notes |
|---|---|---|
| `flux.1-pro` | Best quality, photorealism | Slowest, highest cost |
| `flux.1-dev` | Development and testing | Open weights available |
| `flux.1-schnell` | Fast generation | Apache 2.0, fastest |
| `flux.1-fill-pro` | Inpainting / outpainting | Requires mask |
| `flux.1-canny-pro` | Edge-guided generation | Requires canny edge map |
| `flux.1-depth-pro` | Depth-guided generation | Requires depth map |

## Usage

Natural language — Hermes will invoke the skill automatically for image requests:

```
"Generate a photorealistic image of a mountain lake at sunset"
"Create an illustration of a robot reading a book, flat design style"
"Make a product photo of a white coffee mug on a wooden table"
"Generate a logo concept for a tech startup called NovaSpark"
```

Or load the skill explicitly:

```bash
/skill black-forest-labs-flux
```

## API Reference

**Text-to-image:**

```python
import bfl
result = bfl.generate(
    prompt="A serene mountain lake at golden hour",
    model="flux.1-pro",
    width=1024,
    height=1024,
    steps=28,
    guidance=3.5
)
result.save("output.png")
```

**Image-to-image:**

```python
result = bfl.generate(
    prompt="Same scene but in winter with snow",
    model="flux.1-pro",
    image="input.png",
    strength=0.75
)
```

**Inpainting:**

```python
result = bfl.generate(
    prompt="Replace the background with a starry night sky",
    model="flux.1-fill-pro",
    image="input.png",
    mask="mask.png"
)
```

## Prompt Tips

- **Be specific about style:** "oil painting", "photorealistic", "flat vector", "watercolor", "cinematic"
- **Include lighting:** "golden hour", "soft studio lighting", "dramatic side lighting"
- **Specify aspect ratio intent:** FLUX handles 1:1, 16:9, 9:16, and 4:3 natively via width/height
- **Typography:** FLUX.1 handles text in images better than most models — specify exact text in quotes

## Common Pitfalls

1. **Rate limits.** BFL API has per-minute and per-day rate limits depending on tier. Add retry logic with exponential backoff for batch generation.
2. **Long generation times.** `flux.1-pro` can take 15–30 seconds per image. Use `flux.1-schnell` for rapid iteration, switch to `flux.1-pro` for final output.
3. **Prompt injection.** Avoid user-controlled strings directly in prompts without sanitization if building an app on top.
4. **Image size constraints.** Width and height must be multiples of 32. Values outside 256–2048 will error.
5. **API key scope.** BFL API keys are scoped to a workspace — ensure the key has access to the model variant you're calling.

## Verification Checklist

- [ ] `BFL_API_KEY` set and valid
- [ ] `pip install bfl` completed successfully
- [ ] Test generation returns an image: *"Generate a simple image of a red circle on white background"*
- [ ] Output file is a valid PNG/JPEG that opens correctly
- [ ] Model variant matches your quality/speed requirement
