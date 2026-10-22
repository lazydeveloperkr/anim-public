# Anim Studio — 클로즈드 베타 안내 🎬

Anim Studio를 먼저 사용해 보실 분들을 위한 간단한 안내입니다. AI 에이전트와 함께 스토리보드를 만들고, Kie.ai 또는 로컬 ComfyUI에서 영상으로 제작할 수 있습니다.

아직 개발 중인 버전입니다. 특별한 테스트 과제는 없으니 원하는 영상을 자유롭게 만들어 보세요. 오류나 사용하기 어려운 부분이 있다면 편하게 알려주세요.

[English guide](README.en.md)

## Anim Studio 설치

Anim Studio 베타 설치 파일은 참가자에게 **별도로 전달**됩니다. 전달받은 Windows 설치 파일로 설치해 주세요. 이 저장소에는 앱 소스 코드나 설치 파일이 들어 있지 않습니다.

## 영상 생성 방식

- **Kie.ai:** 이전 UI에서는 정상 동작했습니다. UI 개편 후 아직 다시 테스트하지 못했으므로 일부 동작이 불안정할 수 있습니다. API 사용 시 비용이 발생할 수 있습니다.
- **로컬 ComfyUI:** ComfyUI-EZ-Desktop (또는 호환 ComfyUI 환경)과 모델, Anim Bridge가 준비되어 있어야 합니다. 처음에는 아래 권장 워크플로우로 짧은 영상부터 만들어 보세요.

### ComfyUI Anim Bridge 설치

**공개 저장소:** [ComfyUI-Anim-Bridge](https://github.com/lazydeveloperkr/ComfyUI-Anim-Bridge)

1. ComfyUI와 EZ Desktop을 종료합니다.
2. 위 공개 저장소에서 **Code → Download ZIP**을 선택합니다.
3. ZIP의 압축을 풀고 폴더 이름을 `comfyui_anim_bridge`로 변경합니다.
4. 실제 ComfyUI 설치 경로의 `ComfyUI/custom_nodes/comfyui_anim_bridge`에 넣습니다. 이 폴더 **바로 아래**에 `__init__.py`가 있어야 합니다.
5. ComfyUI 서버를 다시 실행하고 열려 있는 ComfyUI 화면도 새로고침하거나 재실행합니다.

EZ Desktop의 실제 디렉터리는 배포판에 따라 다를 수 있습니다. 상세 설치와 연결 설명은 [Bridge README](https://github.com/lazydeveloperkr/ComfyUI-Anim-Bridge#readme)를 참고해 주세요.

### 추천 워크플로우

**`minimax_h3_r2v_Latent Upscaler-fps-up_Anim`**

[공개 Bridge 저장소의 워크플로우 파일](https://github.com/lazydeveloperkr/ComfyUI-Anim-Bridge/blob/main/workflows/minimax_h3_r2v_Latent%20Upscaler-fps-up_Anim.json)

Bridge의 `workflows` 폴더에 포함된 JSON 파일을 ComfyUI에서 열어 사용합니다. MiniMax H3 참조 영상 생성, Latent Upscaler, RIFE 프레임 보간을 결합한 구성입니다. **모델과 추가 노드가 모두 준비되어 있어야** 실행됩니다.

필요한 주요 모델:
- MiniMax H3 본체: `minimax_h3_ref2va_pruned_int8_convrot.safetensors`
- 텍스트 인코더: `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors`
- Video/Audio VAE: `minimax_h3_video_vae_fp16.safetensors`, `minimax_h3_audio_vae_fp32.safetensors`
- Turbo LoRA: `minimax_h3_ref2v_turbo_4step_v0.1_comfyui_bf16.safetensors`
- Latent Upscaler: `minimax_h3_latent_upscaler_3d_fp16.safetensors`
- RIFE: `rife_v4.26.safetensors`

[MiniMax H3 기본 모델](https://huggingface.co/Comfy-Org/MiniMax-H3) · [MiniMax H3 Latent Upscaler 노드](https://github.com/LBH-123-AI/Comfyui_Minimax_h3_latent_Upscaler) · [KJNodes](https://github.com/kijai/ComfyUI-KJNodes)

ComfyUI 버전과 해당 확장 노드 의존성이 맞아야 합니다. JSON을 열었을 때 *Missing Nodes* 또는 모델 누락 표시가 나오면 해당 노드/모델을 먼저 설치해 주세요. Python 패키지는 ComfyUI가 **실제로 사용하는 환경**에 설치해야 하며, 기존 CUDA/PyTorch 버전을 임의로 교체하지 않는 것이 좋습니다.

먼저 ComfyUI에서 워크플로우가 정상 동작하는지 확인한 다음, Anim 설정에서 ComfyUI 서버 주소를 연결해 주세요. **ComfyUI 창/브라우저 탭과 권장 워크플로우를 열어둔 상태**에서 Anim으로부터 입력을 전송할 수 있습니다.

## AI 에이전트와 함께 사용하기

**Codex 또는 Claude 에이전트 사용을 권장합니다.** Grok Build도 사용 가능합니다. 에이전트에 Anim Studio MCP를 연결한 뒤 스토리보드 기획, Sequence 편집, 영상 프롬프트 작성을 부탁해 보세요. MCP 연결 방법은 사용 중인 에이전트의 설정에 따라 다릅니다.

처음에는 **짧은 Sequence 하나**를 만들어 ComfyUI에 입력만 전송하고 결과를 확인해 보셔도 충분합니다.

## 현재 알려진 문제

- **ChatGPT MCP 터널:** 원격 연결 오류가 있어 사용이 어려울 수 있습니다. 현재는 Codex나 Claude를 권장합니다.
- **Windows 워크플로우 중복 표시:** ComfyUI의 같은 이름 워크플로우가 Anim 목록에 두 번 보이는 현상이 있습니다. ComfyUI 중복 탭/창을 닫고 목록을 새로고침해 보세요. 정확한 원인은 조사 중입니다.
- **Kie.ai:** UI 개편 이전에는 동작했으나 개편 이후에는 재검증되지 않았습니다.

## 사용 중 불편한 점이 있나요?

어느 화면에서 무슨 일이 생겼는지 간단히 알려주시거나 스크린샷을 보내주시면 됩니다. 정해진 점검표나 테스트 보고서를 작성하실 필요는 없습니다.

즐겁게 사용해 주세요! 
