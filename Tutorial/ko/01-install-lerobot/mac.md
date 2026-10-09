[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | 한국어 | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Mac 컴퓨터

검은색 Leader 암은 5V6A 전원 어댑터를 사용합니다.

흰색 Follower 암은 12V5A 전원 어댑터를 사용합니다.

## 권한 부여

![Mac 시스템 설정 창을 보여 주는 이미지로, 현재 접근성(Accessibility) 설정 페이지가 열려 있고 좌측 사이드바에서 개인 정보 보호 및 보안(Privacy & Security)이 선택되어 있습니다. 창에는 Baidu Netdisk, DingTalk, Doubao 등 여러 응용 프로그램이 나열되어 있으며, Terminal 앱의 스위치가 빨간색으로 표시되어 켜져 있습니다. 이는 Mac 워크플로의 "권한 부여" 단계에 해당하며, 이후 Miniconda 설치와 미러 변경을 준비하기 위해 관련 Terminal 권한을 활성화하는 것을 보여 줍니다.](../../en/images/d04-01.png)

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
conda create -y -n lerobot python=3.12
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

![이 이미지는 `ffmpeg` 명령을 실행한 뒤의 터미널 출력을 보여 주며, ffmpeg가 정상적으로 설치되었는지 확인하는 용도로 "ffmpeg 설치" 이후의 확인 단계에 해당합니다. 출력에는 FFmpeg 버전 7.1.1, 2000년부터 2025년까지 FFmpeg 개발자에게 있는 저작권, 구성 플래그, 지원하는 인코더 목록 그리고 Universal Media Converter의 사용 안내가 명확히 표시되어 있으며, 마지막에는 더 자세한 도움말을 보려면 `-h` 옵션이나 `man ffmpeg` 명령을 사용하라는 안내가 나옵니다.](../../en/images/d04-02.png)

## LeRobot 다운로드

- 공식 LeRobot 저장소를 다운로드합니다

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 코드 저장소 설치

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![이 이미지는 Mac에서 pip로 "feetech" 패키지를 설치하는 명령 줄을 보여 줍니다. 설치 진행 상황이 표시되며, "https://repo.huaweicloud.com/repository/pypi/simple/"에서 패키지를 가져오고 datasets, diffusers, huggingface-hub 같은 여러 파일을 내려받는 과정을 거쳐 마지막으로 "einops==0.8.0" 다운로드로 끝납니다. 이 이미지는 "코드 저장소 설치" 절과 관련이 있으며, 저장소를 설치할 때의 명령 실행과 결과를 시각적으로 보여 줍니다.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![이 이미지는 LeRobot 코드 저장소를 설치한 뒤 Mac에서 확인하는 화면을 보여 줍니다. 터미널에는 "Successfully installed LeRobot"가 표시되고 numpy, pandas 등 설치된 여러 Python 패키지와 해당 버전이 나열됩니다. 이 이미지는 "코드 저장소 설치" 및 "설치 확인" 절에 해당하며, 설치된 패키지를 시각적으로 보여 주어 설치 성공 여부를 확인할 수 있게 합니다.](../../en/images/d04-04.png)
</column>
</grid>

## 설치 확인

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![이 이미지는 Mac 터미널 명령 줄의 스크린샷으로, LeRobot 설치 단계에서 구성을 확인하는 과정의 일부입니다. 현재 프로젝트 디렉터리는 lerobot-main이며, python 명령 뒤에 darwin 시스템의 Python 버전 3.10.19로 대화형 Python 환경에 진입합니다. import lerobot, import torch, torch.cuda.is_available() 명령을 차례로 실행했고 결과로 CUDA 사용 가능 여부가 False로 나왔으며, 이어서 import scservo_sdk를 실행했습니다. 이는 확인 단계에 해당하며, LeRobot과 그 종속 요소의 설치 및 구성 상태를 확인하는 데 사용됩니다.](../../en/images/d04-05.png)

![이 이미지는 Mac에서 LeRobot 명령의 터미널 출력을 보여 줍니다. LeRobot 버전 0.4.3, 플랫폼 macOS - 15.6.1 - arm64 - arm - 64bit, Python 버전 3.12.12 등의 정보가 표시됩니다. 또한 Huggingface Hub, Datasets, NumPy, FFmpeg, PyTorch의 버전 정보, PyTorch가 CUDA 지원으로 빌드되었는지 여부, CUDA 버전과 GPU 모델, 마지막으로 LeRobot 스크립트 목록이 나열됩니다. 이 이미지는 확인 맥락에 해당하며, 설치 후의 LeRobot 정보를 보여 줍니다.](../../en/images/d04-06.png)
