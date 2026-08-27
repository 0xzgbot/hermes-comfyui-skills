---
name: comfyui-video
description: Generate video with LTX, Wan 3.0, and MiniMax H3.
version: 2.0.0
author: 0xzgbot
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [comfyui, video-generation, ltx, wan, minimax]
    category: creative
    related_skills:
      - comfyui
      - ltx23
      - ltx25_beat_scripting
      - workflow_ltx_i2v
      - workflow_ltx_first_last_frame
      - wan
      - minimax_h3
      - character_consistency
      - vision_audit_remediation
    config:
      - key: comfyui.primary_url
        description: Default ComfyUI HTTP origin
        default: "http://127.0.0.1:8188"
        prompt: Default ComfyUI URL
      - key: comfyui.secondary_url
        description: Video-GPU ComfyUI origin (optional)
        default: ""
        prompt: Video ComfyUI URL (blank if one instance)
---

# ComfyUI Video Skill

Local and hosted video through ComfyUI: LTX 2.3/2.5, Wan 2.2 (local) / 3.0 (hosted), MiniMax H3. Stills and anchors belong in `comfyui-image` first.

AnimateDiff remains only as a bundled `comfyui` example workflow — not a dialect to mix with LTX/Wan/H3.

## When to Use

- T2V, I2V, first+last-frame, or beat-timed clips
- User names LTX, Wan, Hailuo, MiniMax H3, or "image to video"

Don't use for: stills-only (`comfyui-image`); Comfy install (`comfyui`).

## Prerequisites

- Bundled `comfyui` skill
- Optional media pack clusters: `ltx23`, `wan`, `minimax_h3`, `workflow_ltx_i2v`, `workflow_ltx_first_last_frame`, `ltx25_beat_scripting`, `character_consistency`
- Video origin: `comfyui.secondary_url` or `COMFYUI_SECONDARY` when a second GPU exists; otherwise primary

## How to Run

```
/comfyui-video I2V 6s from outputs/hero.png, slow push-in, no new wardrobe
```

Then `skill_view` **one** family specialist. For dialects table: `skill_view("comfyui-prompting", "references/dialects.md")`.

## Quick Reference

| Family | Local? | Load |
|---|---|---|
| LTX 2.3 I2V | Yes | `workflow_ltx_i2v` + `ltx23` camera specialist |
| LTX first/last | Yes | `workflow_ltx_first_last_frame` |
| LTX 2.5 beats | Yes | `ltx25_beat_scripting` |
| Wan 2.2 | Yes | `wan_prompt_engineering_master` |
| Wan 3.0 / Prime | Hosted | same `wan` cluster; three-layer audio |
| MiniMax H3 | Hosted (IR hosted-only) | `minimax_h3_prompt_engineering_master` |

## Procedure

1. **Anchor.** Prefer a locked still from `comfyui-image`. Completion: PNG path or explicit T2V (no anchor).
2. **One family.** LTX vs Wan vs H3. Load that cluster with `skill_view` — umbrellas (`ltx23`, `wan`, `minimax_h3`) are indexes, not samplers. Completion: specialist name in the turn.
3. **Motion language.** Camera + subject from **that** family. Present tense. H3: official shot blocks / `<d>` dialogue. Wan 3.0: voice / SFX / music as three layers or explicit `no voice lines`. Completion: prompt contains no foreign-family tokens.
4. **Route.** Video graphs on the video instance (`comfyui.secondary_url`) when set. `delegate_task` if stills are rendering elsewhere. Completion: URL matches the GPU role.
5. **Limits.** LTX: max 257 frames, multiple of 8 plus 1. Don't OOM by leaving Flux weights loaded on the same 3090. Completion: frame count + instance named.
6. **Run.** `terminal` → bundled `health_check.py` then `run_workflow.py` (or the hosted API the specialist documents). Completion: clip path.
7. **Audit frames.** `vision_analyze` first, mid, last (and any title card). Motion smear / morph / identity drift → one fix, re-run. Completion: three-frame notes, not a single first-frame glance.
8. **Consistency.** Multi-shot: `skill_view("character_consistency")` before shot 2.

## Pitfalls

- Pasting LTX paragraphs into Wan 3.0 or H3
- Shipping a clip that only looks correct on frame 0
- H3-Context-IR locally (hosted-only) — convert to Base or Full-Reference
- Wan 3.0 inventing VO/BGM — specify layers or disable Auto Polish after `@` labels are exact
- AnimateDiff + LTX in one prompt

## Verification

- Family named before run
- Clip path exists
- `vision_analyze` on at least two timestamps
