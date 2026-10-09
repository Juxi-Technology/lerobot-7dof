[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | 한국어 | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Windows 컴퓨터

검은색 Leader 암은 5V6A 전원 어댑터를 사용합니다.

흰색 Follower 암은 12V5A 전원 어댑터를 사용합니다.

## Miniconda 설치

anaconda.com/download/success

또는 이 링크를 클릭하여 설치 프로그램을 바로 다운로드할 수 있습니다

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![이 이미지는 Windows용 Miniconda3 설치 화면으로, 소프트웨어 버전 py312_24.7.1-0 (64-bit)를 보여 줍니다. 화면에는 두 가지 설치 유형 옵션이 있는데, "Just Me (recommended)"라고 표시된 옵션이 빨간 박스로 강조되어 있으며 현재 선택된 권장 설치 방식이고, 다른 옵션인 "All Users (requires admin privileges)"는 선택되지 않았습니다. 화면 상단에는 Miniconda3의 설치 유형을 선택하라는 안내가 있고, 하단에는 "Back", "Next", "Cancel" 세 개의 버튼이 있습니다. 이 화면은 Miniconda 설치 흐름에서 설치 범위를 확인하는 핵심 단계입니다.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![이 이미지는 Miniconda3 설치 화면의 고급 설치 옵션을 보여 줍니다. "Add Miniconda3 to my PATH environment variable" 옵션이 빨간 박스로 강조되어 있으며, 옆에는 다른 응용 프로그램과 충돌할 수 있어 권장하지 않는다는 설명과 함께 Windows 시작 메뉴에 추가되는 명령 프롬프트 및 PowerShell 메뉴를 대신 사용하라는 안내가 있습니다. 이 이미지는 "conda 미러 변경" 이후 가상 환경을 생성하는 단계와 관련이 있으며, Miniconda를 설치할 때의 구성 참고 자료입니다.](../../en/images/d05-02.png)
</column>
</grid>

## conda 미러 변경

```Shell
# 먼저 기존 미러 설정을 지웁니다(충돌 방지용)
conda config --remove-key channels

# conda의 기본 채널과 자주 쓰는 서드파티 채널을 칭화(Tsinghua) 미러로 교체합니다
# 기본 패키지 채널(main/r/msys2)을 추가합니다
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# 자주 쓰는 서드파티 채널을 추가합니다
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# 다운로드 출처 표시를 켜서 패키지 설치 시 실제 다운로드 주소가 보이게 합니다
conda config --set show_channel_urls yes

# 새 미러가 적용되도록 인덱스 캐시를 지웁니다
conda clean -i

# 현재 구성을 확인합니다(채널이 정상적으로 추가되었는지 검증)
conda config --show-sources
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

![이 이미지는 Windows 명령 줄 창으로, ffmpeg 명령을 실행한 뒤의 확인 결과를 보여 줍니다. 구체적으로 명령 줄은 ffmpeg 버전 7.1.1, 저작권과 빌드 정보, ffmpeg 관련 라이브러리 파일 정보를 출력하고, 하단에는 기본 사용법과 더 많은 도움말을 얻는 방법을 다루는 사용 안내가 있습니다. 이 이미지는 Windows 컴퓨터에 ffmpeg가 정상적으로 설치되었는지 확인하는 데 사용되며, "ffmpeg 설치" 이후의 확인 단계에 해당하고 ffmpeg 설치 완료 후의 실행 상태를 시각적으로 보여 줍니다.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![이것은 Linux 터미널 인터페이스로, conda 관련 명령과 그 실행을 보여 줍니다. 두 가지 핵심 명령이 명확히 표시되어 있습니다. lerobot이라는 가상 환경을 활성화하는 명령 "$ conda activate lerobot"과 현재 활성 환경을 비활성화하는 명령 "$ conda deactivate"입니다. 현재 (base) 환경이 활성화되어 있고, 터미널은 conda-forge 채널에서 ffmpeg 7.1.1을 설치하는 중이며 여러 구성된 미러 주소를 보여 주고, 패키지 메타데이터와 종속 환경 수집 흐름은 이미 완료된 상태입니다.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![이 이미지는 Ubuntu에서 ffmpeg 명령을 사용하는 터미널 인터페이스를 보여 줍니다. 버전 번호, 빌더 및 컴파일러 구성 세부 정보를 포함한 ffmpeg 버전 정보가 표시됩니다. 또한 libavcodec, libavformat 등 여러 코덱의 버전을 나열합니다. 하단에는 사용 안내가 있으며, 모든 도움말을 보려면 "-h"를 사용하거나 "man ffmpeg"를 실행하라고 안내합니다. 이 이미지는 "ffmpeg 설치" 절과 관련이 있으며, 버전과 빌드 정보를 보여 주어 ffmpeg 설치 성공을 확인하는 데 사용됩니다.](../../en/images/d05-05.png)
</column>
</grid>

## LeRobot 다운로드

- 공식 LeRobot 저장소를 다운로드합니다

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 코드 저장소 설치

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![이 이미지는 LeRobot 코드 저장소를 설치한 뒤 Windows cmd 명령 줄에서의 확인 결과를 보여 줍니다. 명령 줄에는 설치가 성공했음을 나타내는 "Successfully built lerobot" 같은 메시지가 표시됩니다. 또한 numpy 1.22.3, scipy 1.7.1 등 여러 Python 패키지와 그 버전 번호를 나열합니다. 하단에는 현재 디렉터리가 Downloads 아래의 lerobot 폴더임을 나타내는 프롬프트 "(lerobot) C:\\Users\\40743\\Downloads\\lerobot>"가 표시됩니다. 이 이미지는 "설치 확인" 절에 해당하며, 설치 성공 후의 명령 줄 피드백을 시각적으로 보여 줍니다.](../../en/images/d05-06.png)

## 설치 확인

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![이 이미지는 Windows의 Python 환경에서 설치 성공을 확인하는 화면을 보여 줍니다. 명령 줄에는 Python 버전 3.10.19가 표시되고, lerobot, scservo_sdk, torch 같은 모듈을 임포트하는 코드를 실행했으며, 마지막으로 torch.cuda.is_available()를 실행하여 False를 반환했습니다. 이 이미지는 "설치 확인" 절에 해당하며, LeRobot 코드 저장소 설치 후 Python 환경을 통해 설치 성공을 확인하는 동작과 결과를 시각적으로 보여 줍니다.](../../en/images/d05-07.png)
