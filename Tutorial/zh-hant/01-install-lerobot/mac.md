[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | 繁體中文 | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# MAC電腦

黑色主動臂使用 5V6A 電源供應器

白色從動臂使用 12V5A 電源供應器

## 設定權限

![這張圖片展示了Mac電腦的系統設定介面，目前處於「輔助使用」的設定頁面，左側選取項為「隱私權與安全性」。介面中列出了多個應用程式選項，包括百度網盤、釘釘、豆包等，其中「終端機」應用程式的開關被紅色框標註，且該開關處於開啟狀態，這對應文件中MAC電腦相關操作流程裡「設定權限」環節的內容，用於開啟終端機的相關權限設定，為後續安裝Miniconda、換源等操作做準備。](../../en/images/d04-01.png)

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
conda create -y -n lerobot python=3.12
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

![這張圖片是終端機執行`ffmpeg`命令後的輸出結果，用於驗證ffmpeg的安裝是否成功，對應文件中「安裝ffmpeg」後的驗證環節。輸出內容清楚顯示了FFmpeg的版本為7.1.1，版權歸2000到2025年的FFmpeg開發者所有，還包含了設定參數、支援的編碼器清單以及Universal Media Converter的使用說明，最後提示可使用`-h`參數或`man ffmpeg`命令取得更多說明資訊。](../../en/images/d04-02.png)

## 下載LeRobot

- 下載LeRobot官方程式庫

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 安裝程式碼儲存庫

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![圖片展示的是在MAC電腦上使用pip安裝「feetech」套件的命令列介面。介面中顯示了安裝套件的進度，包括從「https://repo.huaweicloud.com/repository/pypi/simple/」取得套件，以及下載多個檔案的過程，如datasets、diffusers、huggingface-hub等，最後下載完成「einops==0.8.0」。該圖片與文件中「安裝程式碼儲存庫」部分內容相關，直觀呈現了安裝程式碼儲存庫時的命令執行情況及結果。](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![圖片展示的是在MAC電腦上安裝LeRobot程式碼儲存庫後的驗證安裝成功介面。終端機顯示「Successfully installed LeRobot」，並列出多個已安裝的Python套件及其版本，如numpy、pandas等。該圖片與文件中「安裝程式碼儲存庫」及「驗證安裝成功」部分內容對應，直觀呈現了安裝成功後的套件安裝情況，協助使用者確認安裝是否成功。](../../en/images/d04-04.png)
</column>
</grid>

## 驗證安裝成功

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![這張圖片是MAC電腦終端機的命令列操作介面擷圖，是LeRobot相關安裝步驟中驗證設定的環節。介面顯示目前處於名為lerobot-main的專案目錄，執行了python命令後進入Python互動環境，Python版本為3.10.19，執行系統為darwin。介面中依序執行了import lerobot、import torch、torch.cuda.is_available()等命令，結果顯示torch.cuda的可用性為False，後續還執行了import scservo_sdk的操作。該內容對應文件中驗證安裝成功的步驟，用於確認LeRobot及相關相依套件的安裝與設定狀態。](../../en/images/d04-05.png)

![圖片展示的是在MAC電腦上使用勒洛特（Lerobot）相關命令的終端機輸出結果。顯示了Lerobot版本為0.4.3，平台為macOS - 15.6.1 - arm64 - arm - 64bit，Python版本3.12.12等資訊。還列出了Huggingface Hub、Datasets、NumPy、FFmpeg、PyTorch等版本資訊，以及PyTorch是否帶有CUDA支援、Cuda版本、GPU模型等。最後列出勒洛特指令碼清單。該圖片與文件中驗證安裝成功的上下文對應，用於展示安裝後的Lerobot相關資訊。](../../en/images/d04-06.png)
