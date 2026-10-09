English | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Mac Computer

The black Leader arm uses a 5V6A power adapter.

The white Follower arm uses a 12V5A power adapter.

## Grant Permissions

![This image shows a Mac system settings window, currently on the Accessibility settings page, with Privacy & Security selected in the left sidebar. The window lists several applications, including Baidu Netdisk, DingTalk and Doubao; the switch for the Terminal app is outlined in red and is turned on. This corresponds to the "Grant Permissions" step in the Mac workflow, enabling the relevant Terminal permissions in preparation for installing Miniconda and changing mirrors later.](../../en/images/d04-01.png)

## Install Miniconda

https://www.anaconda.com/download

## Change the pip Mirror

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## Change the conda Mirror

```Shell
# Clear the existing .condarc configuration (optional, to avoid conflicts)
echo "" > ~/.condarc

# Write the Tsinghua mirror configuration
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

# Clear the cache so the configuration takes effect
conda clean -i
```

## Create a Virtual Environment

```Shell
conda create -y -n lerobot python=3.12
```

## Activate the Virtual Environment

```Shell
conda activate lerobot
```

## Install ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

Verify that the installation succeeded

```Shell
ffmpeg
```

![This image shows the terminal output after running the `ffmpeg` command, used to verify that ffmpeg was installed successfully, corresponding to the verification step after "Install ffmpeg". The output clearly shows FFmpeg version 7.1.1, copyright held by the FFmpeg developers from 2000 to 2025, along with configuration flags, the list of supported encoders and usage notes for the Universal Media Converter, ending with a hint to use the `-h` option or the `man ffmpeg` command for more help.](../../en/images/d04-02.png)

## Download LeRobot

- Download the official LeRobot repository

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Install the Code Repository

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![The image shows the command line installing the "feetech" package with pip on a Mac. It displays the installation progress, including fetching packages from "https://repo.huaweicloud.com/repository/pypi/simple/" and downloading several files such as datasets, diffusers and huggingface-hub, finishing with the download of "einops==0.8.0". This image relates to the "Install the Code Repository" section, visually presenting the command execution and result when installing the repository.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![The image shows the verification screen on a Mac after installing the LeRobot code repository. The terminal displays "Successfully installed LeRobot" and lists several installed Python packages with their versions, such as numpy and pandas. This image corresponds to the "Install the Code Repository" and "Verify the Installation" sections, visually presenting the installed packages so users can confirm the installation succeeded.](../../en/images/d04-04.png)
</column>
</grid>

## Verify the Installation

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![This image is a screenshot of the Mac terminal command line, part of verifying the configuration in the LeRobot installation steps. It shows the current project directory lerobot-main; after the python command it enters the interactive Python environment with Python version 3.10.19 on the darwin system. The commands import lerobot, import torch and torch.cuda.is_available() were run in turn, and the result showed CUDA availability as False, followed by import scservo_sdk. This corresponds to the verification step, used to confirm the installation and configuration status of LeRobot and its dependencies.](../../en/images/d04-05.png)

![The image shows the terminal output of a LeRobot command on a Mac. It displays LeRobot version 0.4.3, platform macOS - 15.6.1 - arm64 - arm - 64bit, Python version 3.12.12 and other information. It also lists version information for Huggingface Hub, Datasets, NumPy, FFmpeg and PyTorch, whether PyTorch is built with CUDA support, the CUDA version and GPU model, and finally the list of LeRobot scripts. This image corresponds to the verification context, showing the LeRobot information after installation.](../../en/images/d04-06.png)