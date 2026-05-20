---
name: hermes-comfyui-skills-comfyui-video
description: ComfyUI video generation — LTX 2.3, LTX 2.5, Seedance 2, Wan, AnimateDiff. Video and animation workflows with camera motion and character consistency.
version: 1.0.0
author: zgbot
metadata:
  hermes:
    tags: [comfyui, video-generation, ltx, seedance, wan, animateiff, camera-motion]
---

name: comfyui-video
description: ComfyUI video generation — LTX 2.3, LTX 2.5, Seedance 2, Wan, AnimateDiff. Focused on video and animation workflows with camera motion and character consistency.
skills:
  - comfyui
  - mlops/ltx23-prompt-engineering-master
  - ltx23/camera_movement_language
  - ltx23/subject_motion_performance
  - ltx23/character_consistency
  - mlops/ltx23-prompting-workflow
  - mlops/ltx25-beat-based-scripting
  - mlops/seedance-2-prompt-standard
  - mlops/grok-video-prompting-standard
  - creative/iterative_prompt_refinement
instruction: |
  You are operating the ComfyUI Video Generation Bundle. Focus on video/animation generation.

  Workflow:
  1. Determine target model (LTX 2.3, LTX 2.5, Seedance 2, Wan) and task (T2V, I2V, V2V)
  2. Use the 6-element prompt structure: Era/Genre -> Subject -> Action+Emotion -> Camera Motion -> Lighting -> Stylized Tone
  3. Apply camera motion vocabulary (pan, zoom, track, tilt, crane, dolly)
  4. For characters: apply character consistency protocols across scenes
  5. For human subjects: direction facial performance and body motion
  6. Apply 3-stage sampling: Pass 1 (denoise 1.0) -> Pass 2 (0.65) -> Pass 3 (0.35)
  7. Max 257 frames (multiple of 8 + 1)
  8. For beat-based: use ltx25 beat scripting for musical/video sync
  9. Check ComfyUI health before running
