# Hermes ComfyUI Skills

**ComfyUI playbooks and slash-command bundles** for [Hermes Agent](https://hermes-agent.nousresearch.com). A tap that turns the agent's native image and video tools into a working ComfyUI path: Flux.2 and Z-Image stills, LTX, Wan, and MiniMax H3 video, plus the setup that keeps it local and unattended.

Built for current Hermes (August 2026): YAML frontmatter, Skills Hub taps, and native `vision_analyze` / `image_generate` / `delegate_task` / `cronjob`.

This pack is the operational layer. The cinematography, model dialects, and the larger skill library live in **[hermes-media-skill-pack](https://github.com/0xzgbot/hermes-media-skill-pack)**; the ComfyUI lifecycle (install, nodes, `run_workflow.py`) lives in the bundled **`comfyui`** skill that ships with Hermes. This tap adds the routing and the one-command bundles on top.

## What it does

Five skills, each installed from the tap and loaded by slash command:

| Slug | Slash command | What it does |
|---|---|---|
| `comfyui-prompting` | `/comfyui-prompting` | Model routing and prompt craft for stills and video |
| `comfyui-image` | `/comfyui-image` | Flux.2 and Z-Image stills, inpaint, ControlNet |
| `comfyui-video` | `/comfyui-video` | LTX 2.3/2.5, Wan 2.2/3.0, MiniMax H3 |
| `comfyui-local-shop` | `/comfyui-local-shop` | Isolated `HERMES_HOME` plus multiple Comfy instances |
| `comfyui-batch-cron` | `/comfyui-batch-cron` | Unattended overnight batches via `cronjob` |

Two things the pack insists on:

- **Every render gets a `vision_analyze` gate.** A 200 from Comfy is not a pass; the image has to be looked at.
- **Cloud is a fallback, not a default.** `image_generate` runs only when Comfy is down and the user allows it.

## Install

Hermes discovers skills from YAML frontmatter, then loads bodies with `skill_view(name)`. After installing, start a **new session**

### Skills (Hub tap)

```bash
hermes skills tap add 0xzgbot/hermes-comfyui-skills
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-prompting
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-image
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-video
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-local-shop
hermes skills install 0xzgbot/hermes-comfyui-skills/comfyui-batch-cron
```

Or copy the folders into a project home. With `HERMES_HOME` set, this does not touch `~/.hermes`:

```bash
export HERMES_HOME=/path/to/project/hermes_home
mkdir -p "$HERMES_HOME/skills"
cp -R skills/* "$HERMES_HOME/skills/"
```

### Bundles (one slash command loads a cluster)

Bundles are YAML aliases. They do not install anything; they load skills that are already in `$HERMES_HOME/skills/`, which is why they only work fully once the bundled `comfyui` skill and the media pack are present. Missing names are skipped.

```bash
mkdir -p "${HERMES_HOME:-$HOME/.hermes}/skill-bundles"
cp skill-bundles/*.yaml "${HERMES_HOME:-$HOME/.hermes}/skill-bundles/"
hermes bundles reload
hermes bundles list
```

Then in chat: `/comfyui-video a 6s I2V of the locked hero still`.

If a bundle slug collides with a skill slug, the bundle wins. That is how `/comfyui-image` can load Flux plus Z-Image plus the vision audit together.

## Prerequisites

| Piece | Required? | Notes |
|---|---|---|
| Hermes Agent (current) | Yes | Indexes `name` plus a `description` of 60 characters or fewer |
| Bundled `comfyui` skill | Yes | Ships with Hermes |
| [hermes-media-skill-pack](https://github.com/0xzgbot/hermes-media-skill-pack) | Recommended | Dialects, LTX/Wan/H3 clusters, vision-audit skill |
| ComfyUI instance(s) | For local render | `COMFYUI_PRIMARY` and optional `COMFYUI_SECONDARY` |
| FAL / `image_generate` | Optional | Cloud fallback only when the user allows it |

## How the playbooks work

The playbooks lean on Hermes tools that were not a usable Comfy path in the earlier pack:

- **Progressive disclosure**: `skills_list()` then `skill_view()`, instead of pasting nine SKILL.md files into one turn.
- **`vision_analyze`**: check the PNG or a video frame before calling a render done.
- **`delegate_task`**: stills on one instance, I2V on another, in parallel.
- **`cronjob`**: overnight batches without leaving a chat open.
- **`HERMES_HOME`**: an isolated home so a media project never rewrites `~/.hermes`.
- **Skill config**: Comfy URLs come from `config.yaml` (`skills.config.comfyui.*`), not hardcoded `localhost:8188`.

A typical run: isolate with `/comfyui-local-shop`, pick `/comfyui-image` or `/comfyui-video`, load one dialect with `skill_view` (do not mix dialects), run `run_workflow.py` / `health_check.py` through `terminal`, gate with `vision_analyze`, and let `/comfyui-batch-cron` handle the overnight sweep.

## Layout

```
README.md          This page
SKILLS.md          Tap manifest for skill indexing
CHANGELOG.md       Release history
LICENSE            MIT
skills/
  comfyui-prompting/SKILL.md
  comfyui-image/SKILL.md
  comfyui-video/SKILL.md
  comfyui-local-shop/SKILL.md
  comfyui-batch-cron/SKILL.md
skill-bundles/
  comfyui-*.yaml   One slash command per cluster
```

New skills are simple to add: a `skills/<slug>/SKILL.md` with YAML frontmatter (`name`, `description` ≤ 60 chars) and a matching `skill-bundles/<slug>.yaml` if there is a cluster worth loading at once.

## Status

Current and working on the August 2026 Hermes: tap install, progressive disclosure, vision-gated renders, isolated `HERMES_HOME`, and cron batches. The five skills above are shipped and committed; nothing in this repo is a stub. Model coverage is what the bundled `comfyui` skill and the media pack declare (Flux.2, Z-Image, LTX 2.3/2.5, Wan 2.2/3.0, MiniMax H3), and the bundles load whatever is actually installed, skipping the rest.

See [CHANGELOG.md](CHANGELOG.md) for what changed between the May 2026 pack and this one.

## License

MIT — see [LICENSE](LICENSE).
