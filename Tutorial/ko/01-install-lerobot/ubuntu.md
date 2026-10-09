[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | 한국어 | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Ubuntu 컴퓨터

검은색 Leader 암은 5V6A 전원 어댑터를 사용합니다.

흰색 Follower 암은 12V5A 전원 어댑터를 사용합니다.

## Miniconda 설치

https://www.anaconda.com/download

## pip 미러 변경

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## conda 미러 변경

```Shell
# 기존 .condarc 설정을 지웁니다(선택 사항, 충돌 방지용)
echo "" > ~/.condarc

# 칭화(Tsinghua) 미러 설정을 기록합니다
cat << EOF > ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
EOF

# 설정이 적용되도록 캐시를 지웁니다
conda clean -i
```

## 가상 환경 생성

```Shell
conda create -y -n lerobot python=3.12 -y
```

## 가상 환경 활성화

```Shell
conda activate lerobot
```

## ffmpeg 설치

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

설치가 성공했는지 확인합니다

```Shell
ffmpeg
```

<grid>
<column width-ratio="0.568354">
![이 이미지는 Ubuntu 컴퓨터에서 conda 가상 환경을 활성화하고 ffmpeg를 설치한 결과를 보여 줍니다. 먼저 conda가 lerobot이라는 가상 환경을 활성화한 뒤 conda install ffmpeg=7.1.1 -c conda -forge 명령을 실행하여 conda - forge를 포함한 conda의 Channels 정보를 표시하고, 마지막으로 linux - 64 플랫폼과 완료된 Collecting package metadata 및 Solving environment 작업을 보여 줍니다. 이 이미지는 "ffmpeg 설치" 절에 해당하며, 설치 실행을 시각적으로 보여 줍니다.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![이 이미지는 Ubuntu 시스템 터미널의 스크린샷으로, ffmpeg 명령을 실행한 뒤 반환되는 결과를 보여 줍니다. ffmpeg 버전 7.1.1, 구성 정보, 지원하는 모듈(libavcodec, libavformat 등)의 버전 번호, 그리고 범용 미디어 변환기의 사용 안내가 표시됩니다. 이는 ffmpeg 설치 후의 확인 단계에 해당하며, ffmpeg 도구가 시스템에 정상적으로 설치되었는지 확인하는 데 사용됩니다.](../../en/images/d03-02.png)
</column>
</grid>

## 공식 LeRobot 저장소 다운로드

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 코드 저장소 설치

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![이 이미지는 Ubuntu 시스템 터미널에서 lerobot 디렉터리로 들어가 `pip install -e.\[feetech\]` 명령을 실행하는 과정을 보여 주며, 공식 LeRobot 저장소 설치의 일부입니다. 지정된 Huawei Cloud 저장소에서 패키지를 가져오고, 종속 요소를 설치하며, 관련 데이터셋 패키지(diffusers, huggingface-hub, accelerate 등)를 내려받는 등 명령 실행의 각 단계를 명확히 보여 주며, 여러 패키지에 구체적인 다운로드 진행률, 크기, 속도가 표시되고 마지막에는 종속 요소가 이미 충족되었다는 메시지와 함께 저장소 설치가 완료됩니다.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![이 이미지는 소프트웨어 패키지를 설치하는 동안의 Ubuntu 시스템 터미널 명령 줄 인터페이스를 보여 주며, ffmpeg 같은 소프트웨어를 설치할 때의 종속 요소 처리 정보를 포함합니다. 터미널에는 pytz, pyyaml, numpy 등 처리 중인 패키지 목록과 기존 패키지 버전을 제거하고 새 버전을 설치하는 과정, 그리고 종속 요소 일관성에 대한 설명이 표시됩니다. 이 내용은 "ffmpeg 설치" 이후의 "설치 확인" 단계에 해당하며, ffmpeg 및 기타 소프트웨어의 설치 과정을 검증한 터미널 출력의 기록입니다.](../../en/images/d03-03.png)
</column>
</grid>

## 설치 확인

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## 4090 호스트에서의 결과

![이 이미지는 Ubuntu 컴퓨터에서 실행한 LeRobot 명령과 정보를 보여 줍니다. "Lerobot lerobot -info" 명령은 LeRobot 버전 0.4.3, 플랫폼 Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, Python 버전 3.12.0 등의 정보를 표시합니다. 여기서 PyTorch 버전은 2.7.1 + cu126, CUDA 버전은 12.6, GPU 모델은 NVIDIA GeForce RTX 4090입니다. 이 이미지는 설치 성공 확인과 관련이 있으며, Ubuntu 환경에서 LeRobot의 동작 정보를 보여 줍니다.](../../en/images/d03-04.png)

![이 이미지는 Ubuntu 컴퓨터에서 LeRobot 저장소를 실행하는 동안의 Python 대화형 인터페이스를 보여 줍니다. Python 버전 3.10.12, conda-forge 패키징 정보와 컴파일 시각이 표시됩니다. 사용자가 `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()`, `import scservo_sdk` 명령을 차례로 입력하여 LeRobot 버전 번호 0.4.3, CUDA 사용 가능 여부 True, 그리고 `scservo_sdk`의 성공적인 임포트를 확인했습니다. 이 이미지는 LeRobot 저장소의 설치 확인과 관련이 있으며, 검증 과정을 시각적으로 보여 줍니다.](../../en/images/d03-05.png)

## NVIDIA DGX Spark에서의 결과

![이 이미지는 Ubuntu 컴퓨터에서 LeRobot 저장소를 실행하는 동안의 터미널 출력을 보여 줍니다. LeRobot 버전 0.4.4, CUDA 버전 13.0, GPU 모델 NVIDIA GeForce GTX 1660 Ti가 표시됩니다. 또한 HuggingFace Hub, Datasets, PyTorch 같은 라이브러리 버전과 FFmpeg, PyTorch의 도구 버전을 나열합니다. 마지막으로 LeRobot과 torch의 임포트를 확인하고 torch.cuda.is_available()가 True를 반환하여 CUDA를 사용할 수 있음을 보여 줍니다. 이 이미지는 Ubuntu 컴퓨터에서 LeRobot 저장소를 실행한 결과를 보여 주는 것과 관련이 있습니다.](../../en/images/d03-06.png)
