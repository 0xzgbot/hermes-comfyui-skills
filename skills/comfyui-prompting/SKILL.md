---
name: comfyui-prompting
description: Route Comfy image and video prompting on current Hermes.
version: 2.0.0
author: 0xzgbot
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [comfyui, prompting, flux, ltx, wan, minimax]
    category: creative
    related_skills:
      - comfyui
      - comfyui-image
      - comfyui-video
      - quality_token_sets
      - iterative_prompt_refinement
    config:
      - key: comfyui.primary_url
        description: Primary ComfyUI HTTP origin
        default: "http://127.0.0.1:8188"
        prompt: Primary ComfyUI URL
---

# ComfyUI Prompting Skill

Pick a generator family, write a prompt in **that** family's dialect, then run it through ComfyUI. This skill is routing and craft — not a dump of every media-pack document.

It does **not** replace the bundled `comfyui` lifecycle skill, and it does not invent Seedance or Grok-video recipes.

## When to Use

- User wants an image or video from Comfy and has not locked a model
- Prompt is generic ("cinematic, 8k") and needs a real dialect
- Switching between FLUX 2, Z-Image, LTX, Wan, and MiniMax H3 in one session

Don't use for: Comfy install/node management (`skill_view("comfyui")`); overnight queues (`comfyui-batch-cron`); isolated multi-GPU shops (`comfyui-local-shop`).

## Prerequisites

- Hermes indexes this skill (`name` + `description` in frontmatter)
- Bundled `comfyui` skill present (scripts: `health_check.py`, `run_workflow.py`)
- Optional: [hermes-media-skill-pack](https://github.com/0xzgbot/hermes-media-skill-pack) for dialect specialists
- `skills.config.comfyui.primary_url` or `COMFYUI_PRIMARY`

## How to Run

```
/comfyui-prompting a 6s night-drive I2V from the locked hero still
```

Or stack: `/comfyui /comfyui-prompting` (up to five leading `/skill` tokens).

Load extra doctrine on demand with `skill_view`, including `skill_view("comfyui-prompting", "references/dialects.md")`.

## Quick Reference

| Need | Tool |
|---|---|
| Which dialect? | `read_file` on `references/dialects.md` or `skill_view` as above |
| Comfy up? | `terminal` → bundled `comfyui` `scripts/health_check.py` |
| Run graph | `terminal` → `scripts/run_workflow.py` (from `comfyui`) |
| Cloud stills fallback | `image_generate` only if user allows cloud |
| Judge pixels | `vision_analyze` on the output path |
| Shot list | `todo` |
| Remember shop defaults | `memory` |

## Procedure

1. **Decide family.** Stills → FLUX 2 (hero) or Z-Image (board). Video → LTX (local I2V), Wan 2.2 (local alt), Wan 3.0 / MiniMax H3 (hosted). Completion: one family named; no dual-dialect prompt.
2. **Load doctrine.** `skill_view` the matching media-pack skill if installed (`quality_token_sets`, `iterative_prompt_refinement`, plus the dialect row in `references/dialects.md`). If missing, say so and continue with bundled `comfyui` example workflows. Completion: specialist names listed or "media pack not installed."
3. **Write the prompt in-family.** Present tense for video. Camera, light, and audio tags from that family only. Completion: prompt block ready to inject.
4. **Health then run.** `skill_view("comfyui")` if scripts aren't in context; `terminal` to `health_check.py`, then `run_workflow.py` with API-format JSON. Completion: output file path.
5. **Gate.** `vision_analyze` the still or representative frames. Fail → one change (`iterative_prompt_refinement`), re-run. Completion: pass/fail, not "Comfy returned 200."
6. **Keep what worked.** `memory` for seed/URL/GPU; `skill_manage` if the graph is worth repeating.

## Pitfalls

- Mixing LTX camera language into Wan or H3
- Calling `image_generate` on a fully-local brief
- Treating this SKILL.md as a bundle that auto-loads nine other files — use `skill_view` or install `skill-bundles/comfyui-prompting.yaml`
- Hardcoding `127.0.0.1:8188` when `comfyui.primary_url` is set

## Verification

- `skills_list()` shows `comfyui-prompting` with this description
- One family chosen before any `run_workflow.py`
- Output path exists and was scored with `vision_analyze`
