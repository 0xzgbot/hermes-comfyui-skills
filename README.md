# Hermes ComfyUI Skill Bundles

Curated Hermes Agent skill bundles for ComfyUI image and video generation. One slash command loads everything you need.

## Quick Install

```bash
hermes skills tap add zgbot/hermes-comfyui-skills
hermes bundles list          # see available bundles
```

Or install individual bundles:

```bash
hermes skills install zgbot/hermes-comfyui-skills/comfyui-prompting
hermes skills install zgbot/hermes-comfyui-skills/comfyui-image
hermes skills install zgbot/hermes-comfyui-skills/comfyui-video
```

## Bundles

### `/comfyui-prompting` — All-in-One

Full ComfyUI prompting suite for images and video.

- 9 skills loaded at once
- Flux.2, LTX 2.3/2.5, Seedance 2, SDXL, Wan
- Prompt engineering standards, quality tokens, iterative refinement
- Best for: exploring any ComfyUI generation task

### `/comfyui-image` — Image Focused

Still image generation workflows.

- 4 skills
- Flux.2, SDXL, SDXL img2img, ControlNet, inpainting, upscaling
- Quality token sets + iterative refinement
- Best for: photo generation, character sheets, concept art

### `/comfyui-video` — Video Focused

Video and animation generation workflows.

- 10 skills
- LTX 2.3, LTX 2.5, Seedance 2, Wan, AnimateDiff
- Camera motion language, character consistency, beat-based scripting
- Best for: short-form video, cinematic scenes, music-sync content

## What Are Skill Bundles?

Skill bundles are Hermes-specific YAML files that group multiple skills under a single slash command. Instead of typing `/comfyui /flux2 /ltx23 /quality_tokens` one by one, you type one command and everything loads.

## Prerequisites

These bundles assume you already have the underlying skills installed. They reference skills from:
- `comfyui` (builtin)
- `workflow_flux2_text_to_image` (local)
- `ltx23` (local)
- Various `mlops/` skills (local)

If you're missing skills, the bundle will load what it can and note what's missing.

## License

MIT — use, modify, share.
