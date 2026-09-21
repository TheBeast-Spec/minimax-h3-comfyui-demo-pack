# MiniMax H3 Body Swap — Source-Hand-Preserving Workflow

A ComfyUI workflow for changing a subject's appearance while using the source clip for pose guidance and preserving the source hands. It was built from the **H3 Female Body Swap — Matched Hands v4** graph.

The graph uses front and side reference images to guide the target appearance, DWPose plus MiniMax H3 Fun ControlNet for body/hand/face pose guidance, and SAM3 masks to restrict inpainting to the person while excluding the hands. MiniMax H3 uses the image tags `<Picture 1>` and `<Picture 2>` for the two reference images; see the [official Reference to Video node docs](https://github.com/Comfy-Org/embedded-docs/blob/main/comfyui_embedded_docs/docs/MiniMaxH3ReferenceToVideo/en.md). That hand-mask choice protects the source hands, their gesture, and their original skin tone; this workflow does **not** regenerate the hands. H3 generates the edited person and audio, and the output is composited back over the source scene.

This is an experimental workflow. Clothing, identity, facial expression, hand edges, and lip-sync can drift between runs. Treat the settings below as the tested setup for this graph, not universal best values.

## Before you run it

1. Install or update ComfyUI. This workflow was saved and checked with ComfyUI `0.37.0` and frontend `1.52.7`.
2. Install these custom node packs with ComfyUI Manager, then restart ComfyUI:
   - [ComfyUI-KJNodes](https://github.com/kijai/ComfyUI-KJNodes) — Set/Get buses and utility nodes.
   - [ComfyUI-VideoHelperSuite](https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite) — video input and MP4 output.
   - [MaskVidExperiments](https://github.com/drozbay/MaskVidExperiments) — subject crop, mask cleanup, latent-mask preparation, and uncrop.
   - [ComfyUI-ControlNet-Aux](https://github.com/comfyorg/comfyui-controlnet-aux) — DWPose preprocessor.
3. Download the model files listed below. Do not put model weights or personal media in this repository.
4. Drag `H3_BODY_SWAP_MATCHED_HANDS.json` onto the ComfyUI canvas.
5. Replace the `YOUR_SOURCE_VIDEO.mp4` selection in `VHS_LoadVideo` and the two `YOUR_*_REFERENCE.png` selections in the `LoadImage` nodes with your own files from ComfyUI's `input` folder **before queuing**. The source clip should show one clearly visible person, face, torso, and hands. The graph uses the source video's audio as H3 audio guidance. This conditioning does not guarantee exact lip-sync.
6. Edit the prompt in `MiniMax H3 Reference to Video` for your target. `<Picture 1>` is the front reference and `<Picture 2>` is the side reference. Keep those labels in sync with the two image inputs.

The input placeholders are intentional. The source video and reference images used for the original render are not included.

## Model files

Download from the official [Comfy-Org MiniMax H3 repository](https://huggingface.co/Comfy-Org/MiniMax-H3) and place each file in the matching ComfyUI model folder:

| ComfyUI folder | Required file |
|---|---|
| `models/diffusion_models/` | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` |
| `models/text_encoders/` | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` |
| `models/vae/` | `minimax_h3_video_vae_fp16.safetensors` |
| `models/vae/` | `minimax_h3_audio_vae_fp32.safetensors` |
| `models/loras/` | `minimax_h3_fl2v_turbo_8step_v1.0_comfyui_bf16.safetensors` |
| `models/model_patches/` | `minimax_h3_fun_controlnet_union_pruned_bf16.safetensors` |

The remaining models are:

| ComfyUI folder | Required file | Source |
|---|---|---|
| `models/checkpoints/` | `sam3.1_multiplex_fp16.safetensors` | [Comfy-Org SAM3.1](https://huggingface.co/Comfy-Org/sam3.1) |
| `models/background_removal/` | `birefnet.safetensors` | [Comfy-Org BiRefNet](https://huggingface.co/Comfy-Org/BiRefNet) |
| ControlNet-Aux model folder | `yolox_l.onnx` and `dw-ll_ucoco_384_bs5.torchscript.pt` | Downloaded by or listed in [ComfyUI-ControlNet-Aux](https://github.com/comfyorg/comfyui-controlnet-aux) |

The MiniMax H3 weights are subject to the [MiniMax H3 community license](https://huggingface.co/Comfy-Org/MiniMax-H3). Read the model and node-pack licenses before use.

## Current graph settings

- Source video is normalized to 24 fps and capped at 180 frames.
- DWPose detects hands, body, and face at resolution 768.
- MiniMax H3 Fun ControlNet strength is `0.8`.
- KSampler uses seed `123`, 16 steps, CFG `1`, and denoise `1`.
- The output node saves an H.264 MP4 at 24 fps. Metadata embedding is disabled in this shareable copy.

The graph aligns the generated frame count to MiniMax H3's supported frame grid. H3 video generation is memory-intensive; if you hit an out-of-memory error, lower the crop's `upscale_megapixels` or reduce `frame_load_cap`, then test again. Very short clips may fall outside the model's documented training range; ComfyUI documents the model training range as about 124–362 frames at 24 fps in the [H3 Reference to Video node docs](https://github.com/Comfy-Org/embedded-docs/blob/main/comfyui_embedded_docs/docs/MiniMaxH3ReferenceToVideo/en.md).

## Privacy and use

This repository contains only the workflow and setup instructions. It does not include the original video, reference photos, generated output, model weights, or personal filesystem paths. Use media you have the right to process and share, and get consent before using someone's likeness.
