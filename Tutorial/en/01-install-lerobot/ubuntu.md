English | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Ubuntu Computer

The black Leader arm uses a 5V6A power adapter.

The white Follower arm uses a 12V5A power adapter.

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
conda create -y -n lerobot python=3.12 -y
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

<grid>
<column width-ratio="0.568354">
![The image shows the result of activating a conda virtual environment and installing ffmpeg on an Ubuntu computer. First, conda activates the virtual environment named lerobot, then the command conda install ffmpeg=7.1.1 -c conda -forge is run, showing conda's Channels information including conda - forge, and finally the Platform of linux - 64 along with completed Collecting package metadata and Solving environment operations. This image corresponds to the "Install ffmpeg" section, visually presenting the execution of the installation.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![This image is a screenshot of the Ubuntu system terminal, showing the result returned after running the ffmpeg command. It displays ffmpeg version 7.1.1, its configuration information and the version numbers of the supported modules (such as libavcodec and libavformat), along with usage notes for the universal media converter. It corresponds to the verification step after installing ffmpeg, used to confirm that the ffmpeg tool was installed successfully on the system.](../../en/images/d03-02.png)
</column>
</grid>

## Download the Official LeRobot Repository

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Install the Code Repository

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![This image shows the process of entering the lerobot directory in the Ubuntu system terminal and running the `pip install -e.\[feetech\]` command, which is part of the official LeRobot repository installation. It clearly shows each step of the command execution, including fetching packages from the specified Huawei Cloud repository, installing dependencies and downloading related dataset packages (such as diffusers, huggingface-hub and accelerate), where several packages are marked with their specific download progress, size and speed, ending with a message that the dependencies are already satisfied and completing the repository installation.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![This image shows the command line interface of the Ubuntu system terminal while performing software package installation, including dependency handling information when installing software such as ffmpeg. The terminal displays the list of packages being processed, such as pytz, pyyaml and numpy, as well as the process of uninstalling the existing package versions and installing new ones, with notes about dependency consistency. This content corresponds to the "Verify the Installation" step after "Install ffmpeg", and is a record of the terminal output from verifying the installation process for ffmpeg and other software.](../../en/images/d03-03.png)
</column>
</grid>

## Verify the Installation

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## Results on a 4090 Host

![The image shows the LeRobot commands and information running on an Ubuntu computer. The command "Lerobot lerobot -info" displays LeRobot version 0.4.3, platform Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, Python version 3.12.0 and other information. In it, the PyTorch version is 2.7.1 + cu126, the CUDA version is 12.6, and the GPU model is NVIDIA GeForce RTX 4090. This image relates to the verification of a successful installation, showing the operating information of LeRobot in the Ubuntu environment.](../../en/images/d03-04.png)

![The image shows the Python interactive interface while running the LeRobot repository on an Ubuntu computer. It displays Python version 3.10.12, including conda-forge packaging information and compilation time. The user entered the commands `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()` and `import scservo_sdk` in turn, obtaining the LeRobot version number 0.4.3, CUDA availability True, and successful import of `scservo_sdk`. This image relates to verifying the installation of the LeRobot repository, visually presenting the verification process.](../../en/images/d03-05.png)

## Results on an NVIDIA DGX Spark

![The image shows the terminal output while running the LeRobot repository on an Ubuntu computer. It displays LeRobot version 0.4.4, CUDA version 13.0, and the GPU model NVIDIA GeForce GTX 1660 Ti. It also lists library versions such as HuggingFace Hub, Datasets and PyTorch, as well as tool versions for FFmpeg and PyTorch. Finally it verifies the imports of LeRobot and torch; torch.cuda.is_available() returns True, indicating that CUDA is available. This image relates to running the LeRobot repository on an Ubuntu computer, showing the results.](../../en/images/d03-06.png)