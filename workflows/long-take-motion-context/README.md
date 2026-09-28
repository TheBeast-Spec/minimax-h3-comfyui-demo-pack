# H3 Long Take (Motion Context V2), Noncoder edition

A beginner-friendly ComfyUI workflow for MiniMax H3 reference-to-video long takes. You write a scene on a timeline, split it into segments, and render them into one video with sound. It runs on the multi-track editor from ComfyUI-Easy-Media.

The canvas is split into six numbered areas, each with a side note: Start here, Scene editor, Render, Live preview, Takes, and Save. Press keys **1** to **6** on the canvas to jump between them. The model loaders sit in a folded Engine area at the bottom.

## Import

1. Drag `H3_LONG_TAKE_MOTION_CONTEXT.json` onto the ComfyUI canvas.
2. Install the missing custom nodes from ComfyUI Manager.
3. Download the models below. ComfyUI offers a download button for most of them when they're missing.
4. Read the **Start here** note on the canvas.

## Custom nodes

- [ComfyUI-Easy-Media](https://github.com/yolain/ComfyUI-Easy-Media): multi-track editor, project, and takes
- [ComfyUI-KJNodes](https://github.com/kijai/ComfyUI-KJNodes): live preview, low-VRAM nodes, Set/Get
- [rgthree-comfy](https://github.com/rgthree/rgthree-comfy): LoRA loader, bookmarks, Low-VRAM switch

## Models

| File | ComfyUI folder | Source |
|---|---|---|
| `Minimax-h3_Singularity_ref2va_Pruned_v1.3_int8.safetensors` | `models/diffusion_models` | [WarmBloodAban/Minimax-h3_Singularity](https://huggingface.co/WarmBloodAban/Minimax-h3_Singularity) |
| `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `models/text_encoders` | [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3/tree/main/text_encoders) |
| `minimax_h3_video_vae_fp16.safetensors` | `models/vae` | [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3/tree/main/vae) |
| `minimax_h3_audio_vae_fp32.safetensors` | `models/vae` | [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3/tree/main/vae) |
| `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors` | `models/loras` | [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3/tree/main/loras) |
| `h3-realism-people-t2v-i2v-r2v.safetensors` | `models/loras` | [fal/MiniMax-H3-Realism-People-LoRA](https://huggingface.co/fal/MiniMax-H3-Realism-People-LoRA) |
| `minimax_h3_latent_upscaler_3d_fp16.safetensors` | `models/latent_upscale_models` | [LBH-123-AI/Minimax_h3_latent_Upscaler](https://huggingface.co/LBH-123-AI/Minimax_h3_latent_Upscaler), file `minimax_h3_latent_upscaler_3d_conv_v1_fp16.safetensors`, renamed |
| `taeh3.safetensors` | `models/vae_approx` | [t8star/Taeh3-Comfy](https://huggingface.co/t8star/Taeh3-Comfy), file `taeh3_2d_kijai.safetensors`, renamed |

Keep the 4-step v0.1 turbo LoRA with Singularity. The 8-step LoRA isn't the pairing Singularity was tuned for.

## GPU guide

- **24 GB or more:** 1.0 to 1.4 MP.
- **16 GB:** 1.0 MP. Tested on an RTX 5070 Ti with 32 GB of system RAM.
- **12 GB:** switch on Low-VRAM mode in the Render area and use 0.7 MP. Not tested yet.
- Per segment, keep frames times megapixels under about 540.

The text encoder is NVFP4, which is built for RTX 50 cards. On an RTX 30 or 40 card, try `qwen3vl_32b_minimax_h3_int8_convrot` from [Comfy-Org/MiniMax-H3](https://huggingface.co/Comfy-Org/MiniMax-H3/tree/main/text_encoders). It's a bigger file and hasn't been tested with this workflow.

## Single or dual

**Single** renders each segment once at full size. Use it when segments continue into each other (Context segments), because the joins stay clean.

**Dual** renders each segment small first, then upscales and sharpens it. Use it when segments are separate shots.

Both modes render every segment on the timeline. To render one segment, set `segment_start_number` to its number and `segment_count` to 1.

## Known limits

- H3 sometimes cuts to a different angle partway through a clip. It's random per seed, so render again with a new seed.
- In ComfyUI's newer node style, the Low-VRAM switch shows its toggle without a label. The toggle still works.
- Not tested yet: a render with no reference images, and lip-sync to your own MP3 on the Voice track.

## Source

Adapted from a community MiniMax H3 Motion Context V2 workflow built on the multi-track project from [ComfyUI-Easy-Media](https://github.com/yolain/ComfyUI-Easy-Media) by yolain. The Noncoder edition adds the numbered layout, side notes, takes setup, model links, and Low-VRAM switch. Third-party nodes and model weights remain subject to their own licenses.
