---
name: comfyui-local-shop
description: Run isolated Hermes against multiple Comfy instances.
version: 2.0.0
author: 0xzgbot
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [comfyui, hermes-home, multi-gpu, cinesmith]
    category: creative
    related_skills:
      - isolated-hermes-home
      - multi-comfy-orchestration
      - local-cinematic-pipeline
      - comfyui
      - vision_audit_remediation
    config:
      - key: comfyui.primary_url
        description: Stills / Spark ComfyUI origin
        default: "http://127.0.0.1:8188"
        prompt: Primary ComfyUI URL
      - key: comfyui.secondary_url
        description: Video-GPU ComfyUI origin
        default: "http://127.0.0.1:8189"
        prompt: Secondary ComfyUI URL
---

# ComfyUI Local Shop Skill

Run Comfy production inside a **project `HERMES_HOME`** against **more than one** ComfyUI process (typical: DGX Spark + dual RTX 3090s). Current Hermes profiles, external skill dirs, and `delegate_task` make this possible without rewriting the user's daily-driver `~/.hermes`.

This is not a second Hermes binary. It is isolation + routing.

## When to Use

- Cinesmith / Forge / "don't touch my Hermes"
- Two or more Comfy HTTP ports, mixed GPUs, or stills vs video split
- User wants fully local (no FAL / `image_generate`)

Don't use for: a single hobby Comfy on one port (use `comfyui` + `comfyui-image` / `comfyui-video`).

## Prerequisites

- Hermes CLI on PATH
- Willingness to set `HERMES_HOME` to a project directory
- Each Comfy answers `GET /system_stats`
- Optional media pack: `isolated-hermes-home`, `multi-comfy-orchestration`, `local-cinematic-pipeline`

## How to Run

```
/comfyui-local-shop stills on Spark, I2V on 3090-B, do not touch ~/.hermes
```

Prefer `skill_view("isolated-hermes-home")` and `skill_view("multi-comfy-orchestration")` when the media pack is installed.

## Quick Reference

| Concern | Tool |
|---|---|
| Confirm home | `terminal` → `echo $HERMES_HOME` |
| Instance health | `terminal` against each origin's `/system_stats` |
| Parallel stills + video | `delegate_task` (one worker per instance) |
| Job list | `todo` |
| GPU map | `memory` |
| Pixel gate | `vision_analyze` |

## Procedure

1. **Isolate.** If this is a media project, `HERMES_HOME` must be the project home **before** any skill install. Completion: `HERMES_HOME` is not `~/.hermes` unless the user opted in (`CINESMITH_ALLOW_GLOBAL_HERMES=1`).
2. **Inventory.** Table of `{name, url, gpu, role}` from config (`comfyui.primary_url`, `comfyui.secondary_url`) plus env (`COMFYUI_PRIMARY`, `COMFYUI_SECONDARY`, optional `COMFYUI_SPARK`). Probe each with `terminal`. Completion: live stats per row.
3. **Roles.** Default: stills (FLUX 2 / Z-Image) on Spark or 3090-A; LTX/Wan I2V on 3090-B; one Comfy **process per GPU**. Completion: every upcoming graph has a URL.
4. **Install target.** This tap and the media pack copy into `$HERMES_HOME/skills/`, not the global home. External skill directories are allowed; they are not write-protection — don't `skill_manage` into a shared tree unless asked.
5. **Dispatch.** `todo` the shot list. `delegate_task` stills vs I2V so they don't share a GPU. Completion: workers named with their Comfy URL.
6. **No cloud unless asked.** Do not call `image_generate` on a local-shop brief.
7. **Audit then ship.** `vision_analyze` (or `vision_audit_remediation`). `memory` the GPU map and last good seeds.

## Pitfalls

- `cp` of skills into `~/.hermes` while `HERMES_HOME` is a project tree
- Two graphs on one 3090
- Assuming `localhost:8188` is "the" Comfy
- `delegate_task` children inheriting the wrong `HERMES_HOME`

## Verification

- `echo $HERMES_HOME` in-session matches the project
- Two origins documented if two GPUs exist
- No `image_generate` on a fully-local job
- `~/.hermes/skills` unchanged unless the user asked to install globally
