# Changelog

## 2026-08-27 — current Hermes (v2)

The May 2026 pack does not load on current Hermes. Bundles were a second YAML
block inside SKILL.md, `.gitignore` dropped every `.yaml`, install used the old
`zgbot/` handle, and the skill lists pointed at paths that no longer exist
(`mlops/…`, `creative/quality_token_sets`, Seedance, Grok video).

What changed:

- Valid tap layout (`skills/<slug>/SKILL.md`) with YAML frontmatter, `name`, and
  `description` ≤ 60 characters so `skills_list()` / slash commands index them.
- Real bundle YAML under `skill-bundles/` for `hermes bundles` (copy into
  `$HERMES_HOME/skill-bundles/`).
- Skill lists use live names from the official bundled `comfyui` skill and
  [hermes-media-skill-pack](https://github.com/0xzgbot/hermes-media-skill-pack)
  (FLUX 2, Z-Image, LTX 2.3/2.5, Wan 2.2/3.0, MiniMax H3).
- Progressive disclosure: playbooks call `skill_view` instead of dumping nine
  skills into one document.
- Native Hermes tools that did not exist (or were not the Comfy path) in May:
  `vision_analyze`, `image_generate`, `delegate_task`, `cronjob`, `todo`,
  `memory`, `skill_manage`.
- Isolated `HERMES_HOME` + multi-Comfy routing (`comfyui-local-shop`).
- Unattended overnight batches (`comfyui-batch-cron`).
- Install path is `0xzgbot/hermes-comfyui-skills`. MIT LICENSE file added.

## 2026-05-20 — v1 (obsolete)

Three pseudo-bundles as SKILL.md files. Not indexable or installable on
current Hermes.
