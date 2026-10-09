English | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Windows Computer

The black Leader arm uses a 5V6A power adapter.

The white Follower arm uses a 12V5A power adapter.

## Install Miniconda

anaconda.com/download/success

Or click this link to download the installer directly

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![This image is the Miniconda3 installation screen on Windows, showing software version py312_24.7.1-0 (64-bit). The screen offers two installation type options, in which the option labeled "Just Me (recommended)" is highlighted with a red box and is the currently selected recommended installation method, while the other option, "All Users (requires admin privileges)", is not selected. The top of the screen prompts you to choose an install type for Miniconda3, and the bottom has three buttons: "Back", "Next" and "Cancel". This screen is the key step in the Miniconda installation flow for confirming the installation scope.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![The image shows the advanced installation options on the Miniconda3 installation screen. The option "Add Miniconda3 to my PATH environment variable" is highlighted with a red box, with a note beside it explaining that this is not recommended because it may conflict with other applications, and suggesting the Command Prompt and PowerShell menus added to the Windows Start menu instead. This image relates to the step of creating a virtual environment after "Change the conda Mirror", and is a configuration reference when installing Miniconda.](../../en/images/d05-02.png)
</column>
</grid>

## Change the conda Mirror

```Shell
# First clear the existing mirror configuration (to avoid conflicts)
conda config --remove-key channels

# Replace conda's default channels and common third-party channels with the Tsinghua mirror
# Add the default package channels (main/r/msys2)
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# Add common third-party channels
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# Turn on showing the download source, so the exact download address is displayed when installing packages
conda config --set show_channel_urls yes

# Clear the index cache so the new mirrors take effect
conda clean -i

# View the current configuration (to verify that the channels were added successfully)
conda config --show-sources
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

![This image is the Windows command line window, showing the verification result after running the ffmpeg command. Specifically, the command line outputs ffmpeg version 7.1.1, with copyright and build information, along with information about ffmpeg's associated library files, and usage notes at the bottom covering basic usage and how to get more help. This image is used to verify that ffmpeg was installed successfully on a Windows computer, corresponding to the verification step after "Install ffmpeg", and visually presenting the running state once ffmpeg installation is complete.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![This is the Linux terminal interface, showing conda-related commands and their execution. Two core commands are clearly labeled: the command to activate the virtual environment named lerobot, "$ conda activate lerobot", and the command to deactivate the active environment, "$ conda deactivate". The (base) environment is currently activated, and the terminal is running the installation of ffmpeg 7.1.1 from the conda-forge channel, showing several configured mirror addresses, while the flow of collecting package metadata and dependency environment is already complete.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![The image shows the terminal interface using the ffmpeg command under Ubuntu. It displays ffmpeg version information, including the version number, builder and compiler configuration details. It also lists the versions of various codecs, such as libavcodec and libavformat. Usage notes are at the bottom, prompting you to use "-h" for all help, or to run "man ffmpeg". This image relates to the "Install ffmpeg" section, used to verify a successful ffmpeg installation by showing its version and build information.](../../en/images/d05-05.png)
</column>
</grid>

## Download LeRobot

- Download the official LeRobot repository

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Install the Code Repository

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![The image shows the verification result in the Windows cmd command line after installing the LeRobot code repository. The command line displays messages such as "Successfully built lerobot", indicating the installation succeeded. It also lists several Python packages and their version numbers, such as numpy 1.22.3 and scipy 1.7.1. The bottom shows the prompt "(lerobot) C:\\Users\\40743\\Downloads\\lerobot>", indicating the current directory is the lerobot folder under Downloads. This image corresponds to the "Verify the Installation" section, visually presenting the command line feedback after a successful installation.](../../en/images/d05-06.png)

## Verify the Installation

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![The image shows the screen verifying a successful installation in the Python environment on Windows. The command line displays Python version 3.10.19, ran code importing modules such as lerobot, scservo_sdk and torch, and finally executed torch.cuda.is_available(), which returned False. This image corresponds to the "Verify the Installation" section, visually presenting the operation and result of verifying a successful installation through the Python environment after installing the LeRobot code repository.](../../en/images/d05-07.png)