# MiniMax H3 Ref2VA ComfyUI Workflow

Downloadable ComfyUI workflows for experimenting with MiniMax H3 reference-driven video. This pack accompanies Noncoder's H3 workflow tutorials.

## Start here

Download the repository as a ZIP using GitHub's **Code > Download ZIP**, then extract it. Import `MINIMAX_H3_REF2VA_WORKFLOW.json` by dragging it onto the ComfyUI canvas. For the fictional four-reference exercise, follow [the demo setup guide](demo/README.md). The base workflow stays blank; load your own references and prompt.

The four-reference demo is an equivalent exercise, not an exact reproduction of the tutorial video. Its characters, wardrobe, car artwork, and dialogue are different. Results depend on installed versions, models, seed, settings, and generation variability. That demo has not yet been rendered and validated in ComfyUI.

## Additional workflow: source-hand-preserving body swap

The experimental [body-swap workflow and setup guide](workflows/body-swap-matched-hands/README.md) uses pose guidance from a source video and protects the source hands, preserving their gesture and skin tone. It does not regenerate hands. The source video and reference photos are not included; provide your own media.

## Source

Companion to Noncoder's **MiniMax H3 ComfyUI: Realism, Lip Sync & Latent Upscaling**. The adapted base workflow follows the structure demonstrated in the [original workflow video](https://www.youtube.com/watch?v=ccvG-Z__pHk). See the [Comfy-Org MiniMax H3 model repository](https://huggingface.co/Comfy-Org/MiniMax-H3) for model files. Third-party nodes and model weights remain subject to their own licenses.

## Contents and privacy

Includes a blank base workflow, four generated fictional reference sheets, a demo prompt and setup guide, provenance and checksums, and the additional body-swap workflow. The repository does not include original tutorial film, reference recordings, private body-swap input media, model weights, credentials, or personal filesystem paths. The fictional demo reference sheets are in `demo/reference-images/`.
