# Four-reference dialogue example

This mirrors the tutorial's four-reference setup and fifteen-second alternating-speaker exercise with entirely new characters, images, and dialogue.

| Image slot | File | Prompt label | Role |
| --- | --- | --- | --- |
| Ref Image 1 | `reference-images/ref1_storyboard.png` | `<Picture 1>` | Sixteen moments of one shot; composition and lighting |
| Ref Image 2 | `reference-images/ref2_nora.png` | `<Picture 2>` | Lead identity, front, side, and rear wardrobe views |
| Ref Image 3 | `reference-images/ref3_car.png` | `<Picture 3>` | Six-view vehicle turnaround; car stays out of frame |
| Ref Image 4 | `reference-images/ref4_owen.png` | `<Picture 4>` | Doorman identity, wardrobe, and earpiece |

## Loading

1. Use your existing MiniMax H3 Ref2VA workflow or import the included blank template. Install the missing model files and nodes from trusted sources. This pack has no model weights.
2. Upload and select the four PNG files in the order above. Enable these four image branches and their Set nodes. Bypass unused image branches 5 through 9, including their Set nodes.
3. Paste all of `PROMPT.txt` into Text (Multiline).
4. Keep all audio-reference branches bypassed. Keep the audio VAE, audio decoding, and generated-audio connection to the video output enabled. The model generates the dialogue and ambience. No reference video is needed.
5. Check model and input errors, set the controls below, then queue one test.

## Controls

| Control | First full-script test | Match tutorial resolution |
| --- | --- | --- |
| Duration | 15 seconds | 15 seconds |
| Aspect ratio | 16:9 | 16:9 |
| Resolution megapixels | 0.5 | 1.5 |
| Resolution multiple | 32 | 32 |
| Sampler | Euler | Euler |
| Scheduler | beta | beta |
| BasicScheduler steps | 12 | 12 |
| Denoise | 1.0 | 1.0 |
| SplitSigmas step | 8 | 8 |
| Reference Turbo 8-step LoRA strength | 1.0 | 1.0 |
| Pass 2 scale / Float | 1.0 | 1.0 |

The included template starts at 1.2 MP. The tutorial's 1.5 MP selector displayed 1664 by 928. Adjust resolution before queuing if needed. Scale 1.0 leaves spatial dimensions unchanged and does not produce 4K.

**Second sampling pass:** the template's node 144, `SamplerCustomAdvanced`, is titled `Pass 2 sampler - OFF` and is bypassed by default. To run the intended eight-plus-four sampling sequence, select that sampler in the Pass 2 group and change its mode to Normal rather than Bypass. Check its output connections remain in place. With BasicScheduler set to 12 and SplitSigmas set to 8, Pass 1 uses the first eight intervals and the enabled Pass 2 sampler continues through the remaining four. If you leave node 144 bypassed, there is no second denoising pass; do not describe that default as a completed twelve-step refinement.

The active LoRA selection is `minimax_h3_ref2v_turbo_8step_v1.0_768p_comfyui_bf16.safetensors`. The name does not by itself determine the graph's total sampling steps. Match the model file to your installation and retain the template's model/encoder/video-VAE/audio-VAE selections when compatible.

For a five-second speed check, shorten the script to one brief exchange; setting five seconds while retaining this fifteen-second prompt does not reliably compress all dialogue. For comparisons, keep the seed fixed and change one control at a time. A fixed seed cannot guarantee identical results across environments.

## Storyboard and output checks

The sixteen panels depict the same camera setup, not sixteen shots. The output should be one full-screen two-shot, with no grid. Nora remains on the left and Owen on the right. The character sheets override small facial or wardrobe differences in the storyboard. The car sheet is included to demonstrate the original reference-slot structure; it is not a car-motion test.

Owen's frontal portrait is the authority for the right-ear earpiece: viewer-left in that frontal portrait is his anatomical right. A small pale line remains in the side-profile neck view after accessory correction; treat it as a reference-image imperfection, not a second earpiece or a left-ear placement instruction. The accessory should remain on his right ear in the generated shot. Check this in the rendered output.

Inspect speaker attribution, mouth movement, faces, wardrobe, and background continuity. Prompted identity consistency and lip sync are not guaranteed. The new pack has been checked as files but has not been rendered and validated in ComfyUI yet.
