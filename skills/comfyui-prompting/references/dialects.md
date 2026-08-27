# Model dialects (do not mix)

Load the matching specialist with `skill_view`. Camera verbs, audio tags, and reference syntax are **not** interchangeable across families.

| Family | When | Load |
|---|---|---|
| FLUX 2 | Hero stills, product, portrait | `workflow_flux2_text_to_image`, `flux2dev_prompt_engineering_master` |
| Z-Image / Klein | Fast boards, then promote | `z_image_turbo` |
| LTX 2.3 / 2.5 | Local I2V, beat-timed clips | `ltx23` then a specialist; `ltx25_beat_scripting`; `workflow_ltx_i2v`; `workflow_ltx_first_last_frame` |
| Wan 2.2 | Local MoE I2V / FLF | `wan` → `wan_prompt_engineering_master`, `wan_camera_motion_control`, `wan_audio_direction` |
| Wan 3.0 / Prime | Hosted, up to 30s, native audio, `@` labels | Same `wan` cluster; Auto Polish off once labels are exact |
| MiniMax H3 / Hailuo | Hosted Base vs Full-Reference; IR is hosted-only | `minimax_h3` → `minimax_h3_prompt_engineering_master` |

Do not paste LTX camera paragraphs into Wan or H3. If the media pack is missing, stay on the bundled `comfyui` workflows (Flux Dev, SDXL, Wan T2V examples) and say which dialect you approximated.
