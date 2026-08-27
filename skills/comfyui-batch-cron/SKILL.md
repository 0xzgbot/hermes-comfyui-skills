---
name: comfyui-batch-cron
description: Schedule unattended Comfy batches with cronjob.
version: 2.0.0
author: 0xzgbot
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [comfyui, cron, batch, overnight]
    category: creative
    related_skills:
      - comfyui
      - comfyui-image
      - comfyui-video
      - comfyui-local-shop
    config:
      - key: comfyui.primary_url
        description: ComfyUI origin for scheduled jobs
        default: "http://127.0.0.1:8188"
        prompt: ComfyUI URL for cron batches
---

# ComfyUI Batch Cron Skill

Queue **unattended** ComfyUI sweeps with Hermes `cronjob` so a chat session does not have to stay open. Current Hermes can inject a scheduled prompt, run `run_batch.py` / `run_workflow.py` via `terminal`, and deliver back to the origin surface.

This is not a rewrite of Comfy's own queue. It is Hermes waking up, checking health, running a known API-format graph, and auditing outputs.

## When to Use

- Overnight seed sweeps, board packs, or a shot list already locked
- User says "run this while I'm away" / "cron the batch"

Don't use for: interactive prompt-craft (use `comfyui-prompting`); first-time graphs that have never succeeded once.

## Prerequisites

- A workflow JSON that has **already** succeeded once in this shop
- Bundled `comfyui` scripts (`run_batch.py`, `health_check.py`, `fetch_logs.py`)
- Hermes `cronjob` tool available on this profile
- Output directory that will still exist at fire time
- Isolated shop: set `HERMES_HOME` in the job the same way as interactive sessions

## How to Run

```
/comfyui-batch-cron 8 random seeds of workflows/hero_api.json at 02:00, stills URL
```

Create the job with `cronjob`, not by asking the user to edit crontab by hand.

## Quick Reference

| Step | Tool |
|---|---|
| Lock the graph | `read_file` the API-format JSON; `todo` remaining seeds |
| Health at fire time | `terminal` → `health_check.py` |
| Sweep | `terminal` → `run_batch.py` (`--count`, `--randomize-seed`, `--parallel` within VRAM) |
| Failures | `terminal` → `fetch_logs.py` |
| QA samples | `vision_analyze` a subset, not only job status |
| Remember schedule | `memory` |

## Procedure

1. **Refuse cold graphs.** If this exact workflow has never produced a file on this instance, run it interactively first (`comfyui-image` / `comfyui-video`). Completion: prior output path cited.
2. **Write the job prompt.** Include: workflow path, `--args` JSON, output dir, Comfy URL from `comfyui.primary_url` / `COMFYUI_PRIMARY`, `HERMES_HOME` if isolated, and "on failure run `fetch_logs.py` and stop." Completion: prompt is self-contained (cron has no chat history).
3. **Schedule.** `cronjob` with a real time (`02:00`, `every 6h`, or a cron expr). Deliver to origin. Completion: job id shown to the user.
4. **Cap parallelism.** `--parallel` must fit the GPU. Overnight jobs do not `delegate_task` onto a GPU that might be interactive in the morning unless the user said so.
5. **At fire (the job).** Health check → batch → list output paths → `vision_analyze` first, middle, last files → `memory` seeds that passed. Completion: a short report, not a silent folder dump.
6. **Capture.** If the sweep found a keeper, `skill_manage` a tiny recipe skill (graph path + args + seed).

## Pitfalls

- Scheduling a prompt that says "continue from above" — cron has no above
- Parallelism that OOMs an idle machine at 2am
- Skipping `vision_analyze` because "batch succeeded"
- Cloud `image_generate` inside a local overnight job
- Forgetting `HERMES_HOME` so the job writes into `~/.hermes`

## Verification

- `cronjob` lists the job with the workflow path in the prompt
- Health check is in the prompt before `run_batch.py`
- User received a deliverable (paths + audit notes), not only "ran"
