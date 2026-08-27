# Hermes ComfyUI Skills

ComfyUI **playbooks and slash-command bundles** for [Hermes Agent](https://hermes-agent.nousresearch.com). Current Hermes (August 2026) — YAML frontmatter, Skills Hub taps, native `vision_analyze` / `image_generate` / `delegate_task` / `cronjob`.

This is the operational layer. Cinematography, dialects, and the 138-skill library live in **[hermes-media-skill-pack](https://github.com/0xzgbot/hermes-media-skill-pack)**. Lifecycle (install, nodes, `run_workflow.py`) lives in Hermes' bundled **`comfyui`** skill.

## Why this rewrite

The May 2026 pack does **not** work on current Hermes:

| Then | Now |
|---|---|
| Fake bundle YAML stuffed inside SKILL.md | Real `skill-bundles/*.yaml` + tap-installable SKILL.md |
| `.gitignore` excluded `*.yaml` so bundles never shipped | Bundles are committed |
| `hermes skills tap add zgbot/…` | `0xzgbot/hermes-comfyui-skills` |
| `skills:` listed `mlops/…`, Seedance, Grok video | Live names: `ltx23`, `wan`, `minimax_h3`, `z_image_turbo`, … |
| Descriptions far over 60 chars → dropped from `skills_list()` | ≤ 60 characters, period-terminated |
| Health-check-then-hope | `vision_analyze` gate on every render |

## Quick install

Current Hermes discovers skills from YAML frontmatter, then loads bodies with `skill_view(name)`. After install, start a **new session** (`/reset` or `--now`).

### Skills (Hub tap)

```bash
hermes skills tap add 0xzgbot/hermes-comfyui-skills
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-prompting
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-image
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-video
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-local-shop
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-batch-cron
```

Or copy the folders into a project home (does **not** touch `~/.hermes` if `HERMES_HOME` is set):

```bash
export HERMES_HOME=/path/to/project/hermes_home
mkdir -p "$HERMES_HOME/skills"
cp -R skills/* "$HERMES_HOME/skills/"
```

### Bundles (one slash command loads a cluster)

Bundles are YAML aliases. They do **not** install skills — they load whatever is already in `$HERMES_HOME/skills/` (official `comfyui`, this tap, and the media pack). Missing names are skipped.

```bash
mkdir -p "${HERMES_HOME:-$HOME/.hermes}/skill-bundles"
cp skill-bundles/*.yaml "${HERMES_HOME:-$HOME/.hermes}/skill-bundles/"
hermes bundles reload
hermes bundles list
```

Then in chat: `/comfyui-video a 6s I2V of the locked hero still`

If a bundle slug collides with a skill slug, **the bundle wins**. That is how `/comfyui-image` can load Flux + Z-Image + vision audit together.

## What's inside

| Slug | Slash command | What it does |
|---|---|---|
| `comfyui-prompting` | `/comfyui-prompting` | Model routing + prompt craft for stills and video |
| `comfyui-image` | `/comfyui-image` | Flux.2 / Z-Image stills, inpaint, ControlNet |
| `comfyui-video` | `/comfyui-video` | LTX 2.3/2.5, Wan 2.2/3.0, MiniMax H3 |
| `comfyui-local-shop` | `/comfyui-local-shop` | Isolated `HERMES_HOME` + multi-Comfy + `delegate_task` |
| `comfyui-batch-cron` | `/comfyui-batch-cron` | Unattended overnight batches via `cronjob` |

## Prerequisites

| Piece | Required? | Notes |
|---|---|---|
| Hermes Agent (current) | Yes | Indexes `name` + `description` ≤ 60 chars |
| Bundled `comfyui` skill | Yes | Ships with Hermes — lifecycle + `scripts/run_workflow.py` |
| [hermes-media-skill-pack](https://github.com/0xzgbot/hermes-media-skill-pack) | Recommended | Dialects, LTX/Wan/H3 clusters, vision-audit skill |
| ComfyUI instance(s) | For local render | `COMFYUI_PRIMARY` (and optional `COMFYUI_SECONDARY`) |
| FAL / `image_generate` | Optional | Cloud fallback only when the user allows it |

## New Hermes capabilities this pack uses

These were not a usable Comfy path in the May pack:

1. **Progressive disclosure** — `skills_list()` then `skill_view(name)` / `skill_view(name, path)`. Do not paste nine SKILL.md files into one turn.
2. **`vision_analyze`** — look at the PNG or a video frame. A 200 from Comfy is not a pass.
3. **`image_generate`** — first-class cloud stills. Use only as fallback when Comfy is down **and** the user did not demand fully local.
4. **`delegate_task`** — stills on one instance, I2V on another, in parallel.
5. **`todo`** — shot lists that survive a long render.
6. **`memory`** — persist GPU map, character DNA, last good seeds.
7. **`cronjob`** — overnight batches without leaving a chat open.
8. **`skill_manage`** — after a graph actually works, save the recipe.
9. **`HERMES_HOME` / profiles** — Cinesmith-style isolated home; this pack must not rewrite `~/.hermes` unless asked.
10. **Slash stacking** — `/comfyui /comfyui-video` loads two skills in one message (up to 5). Prefer a bundle for repeats.
11. **Skill config** — Comfy URLs in `config.yaml` (`skills.config.comfyui.*`), not hardcoded `localhost:8188`.
12. **`/learn`** — after a painful debug, `/learn` the working Comfy path instead of re-deriving it.

## Recommended production flow

1. Isolate if this is a media project → `/comfyui-local-shop` (or `isolated-hermes-home` from the media pack).
2. Pick the lane → `/comfyui-image` or `/comfyui-video` (or `/comfyui-prompting` if the model is still undecided).
3. Load the matching dialect with `skill_view` (`ltx23_camera_movement_language`, `wan_prompt_engineering_master`, `minimax_h3_prompt_engineering_master`). **Do not mix dialects.**
4. Run through the bundled `comfyui` scripts (`run_workflow.py` / `health_check.py`) via the `terminal` tool.
5. Gate with `vision_analyze` (or `vision_audit_remediation` if the media pack is installed).
6. Optional: `/comfyui-batch-cron` for overnight sweeps; `skill_manage` to keep the winning graph.

## License

MIT — see [LICENSE](LICENSE).
