---
name: hermes-comfyui-skills
description: ComfyUI playbooks and bundles for current Hermes.
---

# Hermes ComfyUI Skills

Tap: `0xzgbot/hermes-comfyui-skills` (default path `skills/`).

## Install

```bash
hermes skills tap add 0xzgbot/hermes-comfyui-skills
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-prompting
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-image
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-video
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-local-shop
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-batch-cron
```

## Skills

| Directory | Command | Purpose |
|-----------|---------|---------|
| `comfyui-prompting` | `/comfyui-prompting` | Model routing + prompt craft |
| `comfyui-image` | `/comfyui-image` | Flux.2 / Z-Image stills |
| `comfyui-video` | `/comfyui-video` | LTX, Wan, MiniMax H3 |
| `comfyui-local-shop` | `/comfyui-local-shop` | Isolated Hermes + multi-Comfy |
| `comfyui-batch-cron` | `/comfyui-batch-cron` | Overnight `cronjob` batches |

Copy `skill-bundles/*.yaml` into `$HERMES_HOME/skill-bundles/` for one-command clusters. Bundles skip missing skills (install the media pack + bundled `comfyui` for the full set).
