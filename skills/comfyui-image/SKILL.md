---
name: hermes-comfyui-skills-comfyui-image
description: ComfyUI image generation — Flux.2, SDXL, SD3, img2img, ControlNet, inpainting, upscaling.
version: 1.0.0
author: zgbot
metadata:
  hermes:
    tags: [comfyui, image-generation, flux, sdxl, inpainting, upscaling]
---

name: comfyui-image
description: ComfyUI image generation — Flux.2, SDXL, SD3, img2img, ControlNet, inpainting, upscaling. Focused on still image workflows.
skills:
  - comfyui
  - workflow_flux2_text_to_image
  - creative/quality_token_sets
  - creative/iterative_prompt_refinement
instruction: |
  You are operating the ComfyUI Image Generation Bundle. Focus on still image generation.

  Workflow:
  1. Determine model target (Flux.2, SDXL, SD3, etc.) and task (T2I, img2img, inpaint, upscale)
  2. Craft prompt using quality token sets for the target model
  3. Check ComfyUI health before execution
  4. Run via run_workflow.py with proper param injection
  5. FAL image_generate as fallback when ComfyUI is offline
  6. Use iterative refinement for adjustments
