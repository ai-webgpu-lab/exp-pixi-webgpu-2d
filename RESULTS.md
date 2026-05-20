# Results

## 1. 실험 요약
- 저장소: exp-pixi-webgpu-2d
- 커밋 해시: 7b8c4fc
- 실험 일시: 2026-05-20T15:44:55.672Z -> 2026-05-20T15:45:07.677Z
- 담당자: ai-webgpu-lab
- 실험 유형: `graphics`
- 상태: `success`

## 2. 질문
- PixiJS WebGPU 2D baseline으로 넘기기 전에 sprite batch scene load와 frame pacing 보고 경로를 먼저 고정할 수 있는가
- sprite count, atlas count, batch metadata와 fallback state가 graphics 결과 문서에 같이 남는가
- 실제 PixiJS WebGPU renderer 교체 전 deterministic 2D harness로 반복 검증이 가능한가

## 3. 실행 환경
### 브라우저
- 이름: Chrome
- 버전: 147.0.7727.15

### 운영체제
- OS: Linux
- 버전: unknown

### 디바이스
- 장치명: Linux x86_64
- device class: `desktop-high`
- CPU: 16 threads
- 메모리: 32 GB
- 전원 상태: `unknown`

### GPU / 실행 모드
- adapter: navigator.gpu available
- backend: `webgpu`
- fallback triggered: `false`
- worker mode: `main`
- cache state: `warm`
- required features: ["texture-compression-bc"]
- limits snapshot: {"maxTextureDimension2D":8192,"maxBindGroups":4}

## 4. 워크로드 정의
- 시나리오 이름: Pixi 2D Readiness
- 입력 프로필: 240-sprites-4-atlases-12-batches
- 데이터 크기: spriteCount=240; batchCount=12; atlasCount=4; backend=webgpu; fallback=false; automation=playwright-chromium, spriteCount=240; batchCount=12; atlasCount=4; backend=webgpu; fallback=false; realAdapter=fallback(Extension type blend-mode already has a handler); automation=playwright-chromium
- dataset: -
- model_id 또는 renderer: pixi-webgpu-readiness
- 양자화/정밀도: -
- resolution: 960x540
- context_tokens: -
- output_tokens: -

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 2151.5 ~ 3677.8 ms
- init_ms: 28.1 ~ 29.1 ms
- success_rate: 1
- peak_memory_note: 32 GB reported by browser
- error_type: -

### Graphics / Blackhole
- avg_fps: 55.85 ~ 60.19
- p95_frametime_ms: 17.1 ~ 20.9 ms
- scene_load_ms: 28.1 ~ 29.1 ms
- ray_steps: 0
- taa states: false
- fallback states: false
- backends: webgpu

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | Pixi 2D Readiness | webgpu | warm | 55.85 | 20.9 | scene_load=28.1 ms, fallback=false |
| 2 | Pixi 2D Readiness | webgpu | warm | 60.19 | 17.1 | scene_load=29.1 ms, fallback=false |

## 7. 관찰
- Pixi 2D readiness baseline은 backend=webgpu, fallback_triggered=false로 기록됐다.
- graphics summary는 avg_fps=55.85, p95_frametime_ms=20.9, scene_load_ms=28.1였다.
- sprite batching metadata는 spriteCount=240; batchCount=12; atlasCount=4; backend=webgpu; fallback=false; automation=playwright-chromium로 남았다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. Real Adapter vs Deterministic
- adapter: real=pixi-webgpu-834, deterministic=deterministic-three-style
- avg_fps: real=60.19, deterministic=55.85, delta=+4.34
- p95_frametime: real=17.1 ms, deterministic=20.9 ms, delta=-3.8 ms
- scene_load_ms: real=29.1 ms, deterministic=28.1 ms, delta=+1 ms

## 9. 결론
- PixiJS 계열 2D 그래픽스 실험으로 넘어가기 전 sprite batch readiness baseline과 결과 문서가 연결됐다.
- 다음 단계는 deterministic canvas surface를 실제 PixiJS WebGPU renderer로 교체하되 sprite/batch metadata와 graphics metric 구조를 유지하는 것이다.
- 이후 renderer shootout의 2D workload 입력과 app/browser-image 계열 UI stress 기준으로 재사용할 수 있다.

## 10. 첨부
- 스크린샷: ./reports/screenshots/01-pixi-2d-readiness.png, ./reports/screenshots/02-pixi-webgpu-2d-real-pixi.png
- 로그 파일: ./reports/logs/01-pixi-2d-readiness.log, ./reports/logs/02-pixi-webgpu-2d-real-pixi.log
- raw json: ./reports/raw/01-pixi-2d-readiness.json, ./reports/raw/02-pixi-webgpu-2d-real-pixi.json
- 배포 URL: https://ai-webgpu-lab.github.io/exp-pixi-webgpu-2d/
- 관련 이슈/PR: -
