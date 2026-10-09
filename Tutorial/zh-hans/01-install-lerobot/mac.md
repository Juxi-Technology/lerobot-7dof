[English](../en/01-install-lerobot/mac.md) | 简体中文 | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# MAC电脑

黑色主动臂使用 5V6A 电源适配器

白色从动臂使用 12V5A 电源适配器

## 加权限

![这张图片展示了Mac电脑的系统设置界面，当前处于“辅助功能”的设置页面，左侧选中项为“隐私与安全性”。界面中列出了多个应用程序选项，包括百度网盘、钉钉、豆包等，其中“终端”应用的开关被红色框标注，且该开关处于开启状态，这对应文档中MAC电脑相关操作流程里“加权限”环节的内容，用于开启终端的相关权限设置，为后续安装Miniconda、换源等操作做准备。](../../en/images/d04-01.png)

## 安装Miniconda

https://www.anaconda.com/download

## pip换源

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## conda换源

```Shell
# 清空原有 .condarc 配置（可选，避免冲突）
echo "" > ~/.condarc

# 写入清华源配置
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

# 清除缓存使配置生效
conda clean -i
```

## 创建虚拟环境

```Shell
conda create -y -n lerobot python=3.12
```

## 进入虚拟环境

```Shell
conda activate lerobot
```

## 安装ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

验证安装成功

```Shell
ffmpeg
```

![这张图片是终端执行`ffmpeg`命令后的输出结果，用于验证ffmpeg的安装是否成功，对应文档中“安装ffmpeg”后的验证环节。输出内容清晰显示了FFmpeg的版本为7.1.1，版权归2000到2025年的FFmpeg开发者所有，还包含了配置参数、支持的编码器列表以及Universal Media Converter的使用说明，最后提示可使用`-h`参数或`man ffmpeg`命令获取更多帮助信息。](../../en/images/d04-02.png)

## 下载LeRobot

- 下载LeRobot官方代码库

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 安装代码仓库

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![图片展示的是在MAC电脑上使用pip安装“feetech”包的命令行界面。界面中显示了安装包的进度，包括从“https://repo.huaweicloud.com/repository/pypi/simple/”获取包，以及下载多个文件的过程，如datasets、diffusers、huggingface-hub等，最后下载完成“einops==0.8.0”。该图片与文档中“安装代码仓库”部分内容相关，直观呈现了安装代码仓库时的命令执行情况及结果。](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![图片展示的是在MAC电脑上安装LeRobot代码仓库后的验证安装成功界面。终端显示“Successfully installed LeRobot”，并列出多个已安装的Python包及其版本，如numpy、pandas等。该图片与文档中“安装代码仓库”及“验证安装成功”部分内容对应，直观呈现了安装成功后的包安装情况，帮助用户确认安装是否成功。](../../en/images/d04-04.png)
</column>
</grid>

## 验证安装成功

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![这张图片是MAC电脑终端的命令行操作界面截图，是LeRobot相关安装步骤中验证配置的环节。界面显示当前处于名为lerobot-main的项目目录，执行了python命令后进入Python交互环境，Python版本为3.10.19，运行系统为darwin。界面中依次执行了import lerobot、import torch、torch.cuda.is_available()等命令，结果显示torch.cuda的可用性为False，后续还执行了import scservo_sdk的操作。该内容对应文档中验证安装成功的步骤，用于确认LeRobot及相关依赖的安装与配置状态。](../../en/images/d04-05.png)

![图片展示的是在MAC电脑上使用勒洛特（Lerobot）相关命令的终端输出结果。显示了Lerobot版本为0.4.3，平台为macOS - 15.6.1 - arm64 - arm - 64bit，Python版本3.12.12等信息。还列出了Huggingface Hub、Datasets、NumPy、FFmpeg、PyTorch等版本信息，以及PyTorch是否带有CUDA支持、Cuda版本、GPU模型等。最后列出勒洛特脚本列表。该图片与文档中验证安装成功的上下文对应，用于展示安装后的Lerobot相关信息。](../../en/images/d04-06.png)