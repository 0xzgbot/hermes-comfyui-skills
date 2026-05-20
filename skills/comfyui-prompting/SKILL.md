---
name: hermes-comfyui-skills-comfyui-prompting
description: ComfyUI prompting — Flux.2, LTX 2.3/2.5, Seedance, SDXL, Wan. Full image + video generation suite.
version: 1.0.0
author: zgbot
metadata:
  hermes:
    tags: [comfyui, image-generation, video-generation, flux, ltx, seedance, prompting]
---

name: comfyui-prompting
description: ComfyUI prompting — Flux.2, LTX 2.3/2.5, Seedance, SDXL, Wan. Loads the full ComfyUI cluster plus prompt engineering, quality tokens, and iterative refinement.
skills:
  - comfyui
  - workflow_flux2_text_to_image
  - ltx23
  - mlops/ltx23-prompt-engineering-master
  - mlops/ltx23-prompting-workflow
  - mlops/seedance-2-prompt-standard
  - mlops/flux-ltx-prompt-engineering-standard
  - creative/quality_token_sets
  - creative/iterative_prompt_refinement
instruction: |
  You are operating in the ComfyUI Prompting Bundle. The user wants to generate images and video through ComfyUI with expert-level prompting.

  Workflow:
  1. First, understand what the user wants (image, video, model target)
  2. Use the prompt engineering references to craft the optimal prompt
  3. Apply quality token sets for the target model
  4. Use iterative refinement if the result needs tweaking
  5. For video: use LTX 2.3/2.5 or Seedance prompt standards with camera motion language and subject direction
  6. For images: use Flux.2 workflow with proper text-to-image structure
  7. Always check ComfyUI health before running: python3 scripts/health_check.py
  8. Use run_workflow.py for local execution, comfy-cli for lifecycle
  9. FAL image_generate fallback is available if ComfyUI is offline

  Key principles:
  - Be specific about camera angles, lighting, composition
  - Use present tense for video prompts
  - Include era/genre, subject, action/emotion, camera motion, lighting, tone
  - Apply temporal language strategies for motion quality
  - Consider hardware constraints (VRAM, model size)
