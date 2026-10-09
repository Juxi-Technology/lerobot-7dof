[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | 한국어 | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# D-Robotics RDK S100 추론

구체적인 구현 흐름은 이 링크를 참조하세요<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy Full Workflow Document" type="doc"></cite>



## RDK S100/S100P에서 ACT 모델 엔드투엔드 배포

이 절에서는 D-Robotics RDK S100 시리즈 하드웨어에서 ACT 모델의 전체 배포 루프를 안내합니다. 전체 과정에는 세 가지 핵심 단계가 있습니다. **모델 내보내기**, **양자화 컴파일**, **보드에서의 실행**입니다.

<callout emoji="💡">
**사전 준비:**
- **개발 머신(Host):** 1단계와 2단계를 실행하는 데 사용하며, 보통 모델 학습 머신입니다(어느 정도 성능과 Docker 설치가 필요합니다).
- **보드(Edge):** D-Robotics RDK S100/S100P로, 3단계를 실행하는 데 사용합니다.
- **툴체인:** 이 글은 `rdk_LeRobot_tools` 저장소에 의존합니다. 자세한 내용은 [GitHub 저장소](https://github.com/D-Robotics/rdk_LeRobot_tools)를 참조하세요.
</callout>

<callout emoji="🚨">
**중요한 버전 호환성 안내(반드시 읽기):** 현재 `rdk_LeRobot_tools`의 ONNX 내보내기 흐름은 **LeRobot 데이터셋 v2.1**과 완전히 호환됩니다. 최신 v3.0에서는 데이터 구조가 바뀌었기 때문에, 이 절의 작업을 하기 전에 메인 `lerobot` 저장소를 v2.1과 호환되는 특정 커밋으로 전환해 내보내기 흐름이 순조롭게 실행되도록 할 것을 **강력히 권장**합니다. 
*권장 커밋 ID:* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### 1단계: 모델을 ONNX 형식으로 내보내기 💻 (개발 머신에서)

먼저 **PyTorch로 학습한** 모델을 중간 형식(ONNX)으로 내보내야 합니다.



#### **1. 툴체인 저장소 클론**

`lerobot` 작업 디렉터리로 이동해 RDK 전용 툴체인을 클론합니다:

```Bash
cd lerobot

# 1. v2.1 데이터셋과 호환되는 안정 버전으로 전환
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. D-Robotics RDK 전용 툴체인 클론
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. 내보내기 파라미터 설정**

`rdk_LeRobot_tools/bpu_export_config.yaml` 파일을 편집하고 실제 경로에 맞게 구성을 조정합니다:

```YAML
dataset:
  root: "data/so101_pick_place" # 데이터셋의 절대 또는 상대 경로
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # 원본 PyTorch 모델 가중치 경로
type: "nash-e" # 대상 하드웨어 아키텍처. RDK S100은 nash-e / S100P는 nash-m에 대응
```



#### 3. 내보내기 스크립트 실행

```Bash
# ONNX 내보내기(개발 머신)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **성공 지표**: 현재 디렉터리에 `bpu_export_output` 폴더가 생성되며, 그 안에 `build_all.sh` 스크립트와 나중에 필요한 양자화 캘리브레이션 데이터가 들어 있습니다.



### 2단계: BPU 모델 컴파일 🐳 (개발 머신의 Docker 환경에서)

D-Robotics BPU 모델을 양자화하고 컴파일하려면 OpenExplorer(OE) 환경이 필요합니다. 환경을 격리하기 위해 Docker 사용을 권장합니다.



#### **1.** **Docker 환경과 이미지 준비**

개발 머신에 Docker가 설치되어 있는지 확인합니다([공식 설치 안내](https://docs.docker.com/engine/install/)). 권장 CPU 이미지를 다운로드하고 로드합니다:

```Bash
# 다운로드한 오프라인 이미지 아카이브 로드
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. 컴파일 컨테이너 시작**

<callout emoji="⚠️">
**함정 경고**: 모델을 컴파일하려면 대량의 공유 메모리가 필요합니다. 반드시 `--shm-size=15g` 인수를 추가하세요. 그렇지 않으면 IPC 메모리 오류가 매우 발생하기 쉽습니다.
</callout>

개발 머신의 작업 디렉터리(방금 내보낸 폴더가 들어 있는)를 컨테이너에 마운트합니다:

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(참고: `<docker-image-name>`을 `sudo docker images`로 확인한 실제 이미지 이름으로 교체하세요.)



#### **3.** **컨테이너 안에서 컴파일 실행**

컨테이너 안에 들어가면 원클릭 컴파일 스크립트를 실행합니다:

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **빌드 산출물 확인**

컴파일이 끝나면 `bpu_export_output` 아래에 `bpu_output/` 폴더가 생성됩니다. 여기에는 RDK 보드에서 실행하는 데 필요한 모든 핵심 파일이 들어 있습니다:

- `bpu_output/` 디렉터리 구조 보기

  - `BPU_ACTPolicy_TransformerLayers.hbm` (양자화된 모델 파일)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (양자화된 모델 파일)
  - `action_mean.npy` 및 기타 여러 데이터셋 정규화 파라미터
  - `camera1_mean.npy` 및 기타 카메라 통계 파라미터

---

### 3단계: 보드 배포와 추론 🤖 (RDK S100에서)

<callout emoji="📌">
**사전 조건 확인:**
1. RDK 보드에 이미 `D-Robotics/lerobot` 런타임 환경이 구성되어 있고 `hbm_runtime`이 설치되어 있어야 합니다.
2. 이전 단계에서 생성한 `bpu_output/` 폴더 전체가 `scp`, USB 드라이브 등으로 RDK 보드에 완전히 복사되어 있어야 합니다.
3. 기본 원격조작 구성이 이미 완료되어, 팔의 시리얼 포트, 카메라의 USB 포트, 캘리브레이션 파일이 올바르게 구성되어 있어야 합니다.
</callout>



#### **1.** **BPU 가속 추론 실행**

RDK 보드의 터미널에서 툴체인 디렉터리로 이동해 제어 스크립트를 시작합니다:

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ 문제 해결

실제 배포 중에 문제가 생기면 다음 목록과 대조해 확인하세요:

- **팔이 움직이지 않나요?**

  - 장치가 마운트되었는지 확인하세요: 터미널에서 `ls /dev/ttyACM*`를 입력해 팔의 시리얼 포트가 올바른지 확인합니다.
  - 권한을 확인하세요: 추론 스크립트를 `sudo`로 실행해 보거나, 현재 사용자를 `dialout` 그룹에 추가하세요.
- **카메라 스트리밍 오류 / 이미지 이상 / 팔이 제자리에서 떨리나요?**

  - 핫플러그로 인해 카메라 인덱스가 바뀌었는지 확인하고, 코드의 카메라 파라미터가 실제 `/dev/video*`와 일치하는지 점검하세요.
- **개발 머신에서 컨테이너가 생성한 파일을 복사할 때 "권한 부족"이 보고되나요?**

  - Docker가 마운트한 디렉터리에서 생성된 파일은 기본적으로 root 소유입니다. 개발 머신에서 `sudo chown -R $USER:$USER bpu_export_output`을 실행해 해결하세요.
