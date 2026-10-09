[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | 繁體中文 | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Ubuntu電腦

黑色主動臂使用 5V6A 電源供應器

白色從動臂使用 12V5A 電源供應器

## 安裝Miniconda

https://www.anaconda.com/download

## pip換源

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## conda換源

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

## 建立虛擬環境

```Shell
conda create -y -n lerobot python=3.12 -y
```

## 進入虛擬環境

```Shell
conda activate lerobot
```

## 安裝ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

驗證安裝成功

```Shell
ffmpeg
```

<grid>
<column width-ratio="0.568354">
![圖片展示的是Ubuntu電腦上conda啟用虛擬環境並安裝ffmpeg的命令執行結果。先是conda啟用名為lerobot的虛擬環境，接著執行conda install ffmpeg=7.1.1 -c conda -forge命令，顯示了conda的Channels資訊，包括conda - forge等，最後顯示Platform為linux - 64，以及Collecting package metadata和Solving environment等操作完成的資訊。該圖片與文件中「安裝ffmpeg」部分對應，直觀呈現了安裝操作的執行情況。](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![這張圖片是Ubuntu系統終端機的操作介面擷圖，內容是執行ffmpeg命令後返回的結果，顯示了ffmpeg的版本為7.1.1，以及設定資訊、各項支援的模組（如libavcodec、libavformat等）相關版本號，還標註了通用媒體轉換器的使用說明，對應文件中安裝ffmpeg後驗證安裝成功的環節，用於確認ffmpeg工具已成功安裝到系統中。](../../en/images/d03-02.png)
</column>
</grid>

## 下載LeRobot官方程式庫

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 安裝程式碼儲存庫

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![該圖片展示了在Ubuntu系統終端機中，進入lerobot目錄後執行`pip install -e.\[feetech\]`命令的過程，屬於LeRobot官方程式碼儲存庫安裝環節的操作日誌。圖中清楚顯示了命令執行的各個步驟，包括從指定的華為雲倉庫取得安裝套件、安裝相依套件、下載相關資料集套件（如diffusers、huggingface-hub、accelerate等），其中多個安裝套件標註了具體的下載進度、大小和速度，最終提示相依性已滿足安裝要求，完成了程式碼儲存庫的安裝相關操作。](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![這張圖片展示的是在Ubuntu系統終端機中執行軟體套件安裝相關操作的命令列介面內容，其中包含了安裝ffmpeg等軟體時的相依性處理資訊。終端機內顯示了正在處理的套件清單，如pytz、pyyaml、numpy等，還包含了卸載現有軟體套件版本、安裝新版本的操作過程，同時標註了相依性一致性相關的說明，這一內容對應文件中「安裝ffmpeg」後「驗證安裝成功」的相關操作環節，是驗證ffmpeg等軟體安裝流程的終端機輸出記錄。](../../en/images/d03-03.png)
</column>
</grid>

## 驗證安裝成功

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## 在4090主機上執行的結果

![圖片展示的是在Ubuntu電腦上執行的Lerobot相關命令及資訊。命令「Lerobot lerobot -info」顯示了Lerobot版本為0.4.3，平台為Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35，Python版本3.12.0等資訊。其中，PyTorch版本為2.7.1 + cu126，CUDA版本12.6，GPU模型為NVIDIA GeForce RTX 4090。該圖片與文件中驗證安裝成功的內容相關，用於展示Lerobot在Ubuntu環境下的執行資訊。](../../en/images/d03-04.png)

![圖片展示了在Ubuntu電腦上執行LeRobot程式碼儲存庫時的Python互動介面。介面顯示Python版本為3.10.12，包含conda-forge封裝資訊及編譯時間等。使用者依序輸入了`import lerobot`、`lerobot.__version__`、`import torch`、`torch.cuda.is_available()`、`import scservo_sdk`等命令，分別取得了LeRobot版本號0.4.3、CUDA可用性為True、以及匯入`scservo_sdk`成功的資訊。該圖片與文件中驗證LeRobot程式碼儲存庫安裝成功的內容相關，直觀呈現了驗證過程。](../../en/images/d03-05.png)

## 在輝達DGX Spark上執行的結果

![圖片展示的是在Ubuntu電腦上執行Lerobot程式碼儲存庫時的終端機輸出結果。顯示Lerobot版本為0.4.4，CUDA版本為13.0，GPU模型為NVIDIA GeForce GTX 1660 Ti。還列出了HuggingFace Hub、Datasets、PyTorch等程式庫版本，以及FFmpeg、PyTorch等工具版本。最後驗證了Lerobot和torch的匯入情況，torch.cuda.is_available()返回True，表示CUDA可用。該圖片與文件中在Ubuntu電腦上執行Lerobot程式碼儲存庫的內容相關，展示了執行結果。](../../en/images/d03-06.png)
