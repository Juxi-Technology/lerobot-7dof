[English](../en/01-install-lerobot/windows.md) | 简体中文 | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Windows电脑

黑色主动臂使用 5V6A 电源适配器

白色从动臂使用 12V5A 电源适配器

## 安装Miniconda

anaconda.com/download/success

或者直接点这个链接下载安装包

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![这张图片是Windows系统下Miniconda3的安装界面，显示软件版本为py312_24.7.1-0（64-bit）。界面提供两种安装类型选项，其中标注为“Just Me (recommended)”的选项被红色方框突出显示，是当前选中的推荐安装方式，另一种可选的“All Users (requires admin privileges)”未被选中。界面顶部提示需为Miniconda3的安装选择类型，底部设有“Back”“Next”和“Cancel”三个操作按钮，该界面是Miniconda安装流程中确认安装范围的关键步骤。](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![图片展示的是Miniconda3安装界面中的高级安装选项。其中“Add Miniconda3 to my PATH environment variable”选项被红色框突出显示，旁边有提示说明不推荐添加，因可能与其他应用冲突，建议使用Windows开始菜单中添加的命令提示符和PowerShell菜单。该图片与上文“conda换源”后创建虚拟环境的操作步骤相关，是安装Miniconda时的配置参考。](../../en/images/d05-02.png)
</column>
</grid>

## conda换源

```Shell
# 先清空原有源配置（避免冲突）
conda config --remove-key channels

# 将 conda 的默认源和常用第三方源替换为清华镜像
# 添加默认包源（main/r/msys2）
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# 添加常用第三方源
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# 开启显示下载源，安装包时会显示具体的下载地址
conda config --set show_channel_urls yes

# 清除索引缓存，使新源生效
conda clean -i

# 查看当前配置（验证源是否添加成功）
conda config --show-sources
```

## 创建虚拟环境

```Shell
conda create -y -n lerobot python=3.12
```

## 激活虚拟环境

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

![这张图片是Windows系统的命令行窗口，显示了执行ffmpeg命令后的验证结果，具体内容为命令行输出了ffmpeg的版本信息为7.1.1，附带版权与编译信息，同时展示了ffmpeg关联的库文件信息，底部还标注了ffmpeg的使用说明，包括基本用法和获取更多帮助的方式。该图片用于验证Windows电脑上ffmpeg的安装是否成功，对应文档中“安装ffmpeg”步骤后的验证环节，能直观呈现ffmpeg安装完成后的运行状态。](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![这是Linux终端的操作界面，展示了conda相关操作的指令与执行过程。界面中明确标注了两个核心指令，分别是激活名为lerobot的虚拟环境的指令“$ conda activate lerobot”，以及关闭活跃环境的指令“$ conda deactivate”。当前处于(base)基础环境已激活的状态，终端正执行安装ffmpeg 7.1.1版本的操作，指定从conda-forge渠道安装，同步显示了配置的多个软件源地址，同时能看到软件包元数据与依赖环境的收集流程已完成。](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![图片展示的是在Ubuntu系统下使用ffmpeg命令的终端界面。显示了ffmpeg版本信息，包括版本号、编译者、编译器等配置详情。还列出了多种编解码器的版本，如libavcodec、libavformat等。底部有使用说明，提示使用“-h”获取全部帮助，或运行“man ffmpeg”。该图片与文档中“安装ffmpeg”部分相关，用于验证ffmpeg安装成功，显示其版本及编译信息。](../../en/images/d05-05.png)
</column>
</grid>

## 下载LeRobot

- 下载LeRobot官方代码库

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 安装代码仓库

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![图片展示的是在Windows系统cmd命令行中安装LeRobot代码仓库后的验证结果。命令行显示“Successfully built lerobot”等信息，表明安装成功。还列出多个Python包及其版本号，如numpy 1.22.3、scipy 1.7.1等。底部提示“(lerobot) C:\\Users\\40743\\Downloads\\lerobot>”，表明当前目录为Downloads文件夹下的lerobot文件夹。该图片与文档中“验证安装成功”部分对应，直观呈现了安装成功后的命令行反馈。](../../en/images/d05-06.png)

## 验证安装成功

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![图片展示的是在Windows系统下Python环境验证安装成功的界面。命令行中显示Python版本为3.10.19，运行了导入lerobot、scservo_sdk、torch等模块的代码，最后执行torch.cuda.is_available()返回False。该图片与文档中“验证安装成功”部分对应，直观呈现了安装LeRobot代码仓库后，通过Python环境验证安装是否成功的操作及结果。](../../en/images/d05-07.png)