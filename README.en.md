# Anim Studio — Closed Beta Guide 🎬

Welcome to the Anim Studio closed beta! You can plan videos as storyboards with an AI agent, then generate footage through Kie.ai or a local ComfyUI setup.

This is still an in-development build. There is **no formal testing assignment**—just try making something you like. If anything feels confusing or breaks, a quick message or screenshot is plenty.

[한국어 안내](README.md)

## Install Anim Studio

The **Windows beta installer will be provided separately** to invited testers. Install the app using that package. This repository does not distribute the app's source code or installer.

## Choose a video generation option

- **Kie.ai:** It worked with the previous interface, but it has **not yet been retested after the UI redesign**. Some features may be unstable. API usage may incur charges.
- **Local ComfyUI:** Requires ComfyUI-EZ-Desktop (or a compatible ComfyUI installation), the necessary models, and Anim Bridge. We recommend starting with a short clip using the workflow below.

### Install ComfyUI Anim Bridge

**Public repository:** [ComfyUI-Anim-Bridge](https://github.com/lazydeveloperkr/ComfyUI-Anim-Bridge)

1. Close ComfyUI and EZ Desktop.
2. On the public repository, choose **Code → Download ZIP**.
3. Extract the ZIP and rename the extracted folder to `comfyui_anim_bridge`.
4. Place it under your active ComfyUI installation at `ComfyUI/custom_nodes/comfyui_anim_bridge`. The file `__init__.py` must be **directly inside** that folder.
5. Restart the ComfyUI server and reload/reopen the ComfyUI frontend.

The actual directory may vary by desktop distribution. See the [Bridge README](https://github.com/lazydeveloperkr/ComfyUI-Anim-Bridge#readme) for details.

### Recommended workflow

**`minimax_h3_r2v_Latent Upscaler-fps-up_Anim`**

[Workflow JSON in the public Bridge repository](https://github.com/lazydeveloperkr/ComfyUI-Anim-Bridge/blob/main/workflows/minimax_h3_r2v_Latent%20Upscaler-fps-up_Anim.json)

Open the JSON from the Bridge repository's `workflows` folder in ComfyUI. It combines MiniMax H3 reference-to-video generation, latent upscaling, and RIFE frame interpolation. **It will not run until its model and custom-node dependencies are installed.**

Main required model files:
- MiniMax H3 diffusion: `minimax_h3_ref2va_pruned_int8_convrot.safetensors`
- Text encoder: `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`
- Video/audio VAEs: `minimax_h3_video_vae_fp16.safetensors`, `minimax_h3_audio_vae_fp32.safetensors`
- Turbo LoRA: `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors`
- Latent upscaler: `minimax_h3_latent_upscaler_3d_fp16.safetensors`
- RIFE: `rife_v4.26.safetensors`

[Base MiniMax H3 models](https://huggingface.co/Comfy-Org/MiniMax-H3) · [MiniMax H3 Latent Upscaler nodes](https://github.com/LBH-123-AI/Comfyui_Minimax_h3_latent_Upscaler) · [KJNodes](https://github.com/kijai/ComfyUI-KJNodes)

You also need a ComfyUI version that supports the stock workflow nodes and the dependencies required by the extra nodes. If you see *Missing Nodes* or missing-model warnings, resolve those in ComfyUI first. Install Python dependencies into the environment actually running ComfyUI; avoid replacing your working CUDA/PyTorch setup blindly.

Try running the workflow directly in ComfyUI first. Then configure your ComfyUI server address in Anim. **Keep the ComfyUI frontend and the target workflow open** so Anim Bridge can apply inputs to the workflow.

## Use an AI agent

We recommend **Codex or Claude**. **Grok Build** is also an option. Once the agent is connected to Anim Studio through MCP, you can ask it to plan a storyboard, edit sequences, and write video prompts. The exact MCP setup varies by agent.

For a first try, **one short sequence** is more than enough. You can send the inputs to ComfyUI without automatically starting generation, then check whether everything looks right.

## Known issues

- **ChatGPT MCP tunnel:** Remote tunnel connectivity is currently unreliable. Use Codex or Claude for now.
- **Duplicate workflows on Windows:** The same ComfyUI workflow name may appear twice in Anim. Try closing duplicate ComfyUI tabs/windows and refreshing the list. The root cause is still under investigation.
- **Kie.ai:** Previously working, but not yet regression-tested following the UI redesign.

## Found something odd?

Just tell us which screen you were on and what happened—or send a screenshot. No formal test report or checklist needed.

Enjoy creating!
