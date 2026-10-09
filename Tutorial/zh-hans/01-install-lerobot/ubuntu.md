[English](../en/01-install-lerobot/ubuntu.md) | 简体中文 | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Ubuntu电脑

黑色主动臂使用 5V6A 电源适配器

白色从动臂使用 12V5A 电源适配器

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
conda create -y -n lerobot python=3.12 -y
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

<grid>
<column width-ratio="0.568354">
![图片展示的是Ubuntu电脑上conda激活虚拟环境并安装ffmpeg的命令执行结果。先是conda激活名为lerobot的虚拟环境，接着执行conda install ffmpeg=7.1.1 -c conda -forge命令，显示了conda的Channels信息，包括conda - forge等，最后显示Platform为linux - 64，以及Collecting package metadata和Solving environment等操作完成的信息。该图片与文档中“安装ffmpeg”部分对应，直观呈现了安装操作的执行情况。](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![这张图片是Ubuntu系统终端的操作界面截图，内容是执行ffmpeg命令后返回的结果，显示了ffmpeg的版本为7.1.1，以及配置信息、各项支持的模块（如libavcodec、libavformat等）相关版本号，还标注了通用媒体转换器的使用说明，对应文档中安装ffmpeg后验证安装成功的环节，用于确认ffmpeg工具已成功安装到系统中。](../../en/images/d03-02.png)
</column>
</grid>

## 下载LeRobot官方代码库

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 安装代码仓库

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![该图片展示了在Ubuntu系统终端中，进入lerobot目录后执行`pip install -e.\[feetech\]`命令的过程，属于LeRobot官方代码仓库安装环节的操作日志。图中清晰显示了命令执行的各个步骤，包括从指定的华为云仓库获取安装包、安装依赖、下载相关数据集包（如diffusers、huggingface-hub、accelerate等），其中多个安装包标注了具体的下载进度、大小和速度，最终提示依赖已满足安装要求，完成了代码仓库的安装相关操作。](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![这张图片展示的是在Ubuntu系统终端中执行软件包安装相关操作的命令行界面内容，其中包含了安装ffmpeg等软件时的依赖处理信息。终端内显示了正在处理的软件包列表，如pytz、pyyaml、numpy等，还包含了卸载现有软件包版本、安装新版本的操作过程，同时标注了依赖一致性相关的说明，这一内容对应文档中“安装ffmpeg”后“验证安装成功”的相关操作环节，是验证ffmpeg等软件安装流程的终端输出记录。](../../en/images/d03-03.png)
</column>
</grid>

## 验证安装成功

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## 在4090主机上运行的结果

![图片展示的是在Ubuntu电脑上运行的Lerobot相关命令及信息。命令“Lerobot lerobot -info”显示了Lerobot版本为0.4.3，平台为Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35，Python版本3.12.0等信息。其中，PyTorch版本为2.7.1 + cu126，CUDA版本12.6，GPU模型为NVIDIA GeForce RTX 4090。该图片与文档中验证安装成功的内容相关，用于展示Lerobot在Ubuntu环境下的运行信息。](../../en/images/d03-04.png)

![图片展示了在Ubuntu电脑上运行LeRobot代码库时的Python交互界面。界面显示Python版本为3.10.12，包含conda-forge打包信息及编译时间等。用户依次输入了`import lerobot`、`lerobot.__version__`、`import torch`、`torch.cuda.is_available()`、`import scservo_sdk`等命令，分别获取了LeRobot版本号0.4.3、CUDA可用性为True、以及导入`scservo_sdk`成功的信息。该图片与文档中验证LeRobot代码库安装成功的内容相关，直观呈现了验证过程。](../../en/images/d03-05.png)

## 在英伟达DGX Spark上运行的结果

![图片展示的是在Ubuntu电脑上运行Lerobot代码库时的终端输出结果。显示Lerobot版本为0.4.4，CUDA版本为13.0，GPU模型为NVIDIA GeForce GTX 1660 Ti。还列出了HuggingFace Hub、Datasets、PyTorch等库版本，以及FFmpeg、PyTorch等工具版本。最后验证了Lerobot和torch的导入情况，torch.cuda.is_available()返回True，表明CUDA可用。该图片与文档中在Ubuntu电脑上运行Lerobot代码库的内容相关，展示了运行结果。](../../en/images/d03-06.png)