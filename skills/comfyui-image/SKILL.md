---
name: comfyui-image
description: Generate stills with Flux.2, Z-Image, and Comfy.
version: 2.0.0
author: 0xzgbot
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [comfyui, image-generation, flux, z-image]
    category: creative
    related_skills:
      - comfyui
      - workflow_flux2_text_to_image
      - z_image_turbo
      - quality_token_sets
      - iterative_prompt_refinement
      - vision_audit_remediation
    config:
      - key: comfyui.primary_url
        description: Stills ComfyUI HTTP origin
        default: "http://127.0.0.1:8188"
        prompt: Stills ComfyUI URL
---

# ComfyUI Image Skill

Still-image generation on ComfyUI: FLUX 2 hero frames, Z-Image boards, img2img, inpaint, ControlNet, upscale. Video belongs in `comfyui-video`.

## When to Use

- Text-to-image, img2img, inpaint, ControlNet, or upscale
- Character sheets, product heroes, concept stills, I2V anchors
- User names Flux, FLUX 2, Z-Image, SDXL, or "stills instance"

Don't use for: I2V / T2V (use `comfyui-video`); installing Comfy (bundled `comfyui`).

## Prerequisites

- Bundled `comfyui` skill (workflows + `run_workflow.py`)
- Optional media pack: `workflow_flux2_text_to_image`, `z_image_turbo`, `quality_token_sets`, `vision_audit_remediation`
- Stills origin: `skills.config.comfyui.primary_url` or `COMFYUI_PRIMARY`

## How to Run

```
/comfyui-image hero still, anamorphic, locked wardrobe, 9:16
```

`skill_view("workflow_flux2_text_to_image")` when the media pack is installed.

## Quick Reference

| Task | Path |
|---|---|
| Hero still | FLUX 2 via `workflow_flux2_text_to_image` |
| Fast board | `z_image_turbo`, then promote |
| Img2img / inpaint / upscale | Bundled `comfyui` example workflows |
| Batch stills | `delegate_task` per seed **or** `run_batch.py` via `terminal` |
| Cloud fallback | `image_generate` only if Comfy is down and user allows |
| QA | `vision_analyze` (or `vision_audit_remediation`) |

## Procedure

1. **Target.** T2I vs img2img vs inpaint vs upscale; FLUX 2 vs Z-Image vs bundled SDXL. Completion: one graph family.
2. **Prompt.** `skill_view("quality_token_sets")` if present. Lock identity tokens before adjectives. Completion: prompt + negative + seed policy.
3. **Health.** `terminal` on bundled `comfyui` `scripts/health_check.py` against the **stills** URL. Completion: reachable `/system_stats`.
4. **Run.** `terminal` → `run_workflow.py` with API-format JSON, `--output-dir` set. For many seeds, `delegate_task` workers **or** `run_batch.py` — do not load Flux weights on the video GPU if `comfyui-local-shop` split them. Completion: file path(s).
5. **Audit.** `vision_analyze` identity, hands, text, product. Fail → one change via `iterative_prompt_refinement`. Completion: pass table or named retry.
6. **Fallback.** Only if health failed and the user did not require local: `image_generate`, then still `vision_analyze` the download.

## Pitfalls

- Running 257-frame video graphs on the stills instance
- Shipping because the PNG exists — un-audited stills are not done
- `image_generate` does not take reference images; encode identity in text
- Skipping `todo` on a character sheet (many angles drift)

## Verification

- Output PNG exists on disk
- `vision_analyze` ran on that path
- Family was FLUX 2, Z-Image, or a named bundled workflow — not "whatever Comfy had open"
