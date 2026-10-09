[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | 繁體中文 | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Windows電腦

黑色主動臂使用 5V6A 電源供應器

白色從動臂使用 12V5A 電源供應器

## 安裝Miniconda

anaconda.com/download/success

或者直接點這個連結下載安裝檔

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![這張圖片是Windows系統下Miniconda3的安裝介面，顯示軟體版本為py312_24.7.1-0（64-bit）。介面提供兩種安裝類型選項，其中標註為「Just Me (recommended)」的選項被紅色方框醒目顯示，是目前選取的建議安裝方式，另一種可選的「All Users (requires admin privileges)」未被選取。介面頂部提示需為Miniconda3的安裝選擇類型，底部設有「Back」「Next」和「Cancel」三個操作按鈕，該介面是Miniconda安裝流程中確認安裝範圍的關鍵步驟。](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![圖片展示的是Miniconda3安裝介面中的進階安裝選項。其中「Add Miniconda3 to my PATH environment variable」選項被紅色框醒目顯示，旁邊有提示說明不建議新增，因可能與其他應用程式衝突，建議使用Windows開始功能表中新增的命令提示字元和PowerShell功能表。該圖片與上文「conda換源」後建立虛擬環境的操作步驟相關，是安裝Miniconda時的設定參考。](../../en/images/d05-02.png)
</column>
</grid>

## conda換源

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

## 建立虛擬環境

```Shell
conda create -y -n lerobot python=3.12
```

## 啟用虛擬環境

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

![這張圖片是Windows系統的命令列視窗，顯示了執行ffmpeg命令後的驗證結果，具體內容為命令列輸出了ffmpeg的版本資訊為7.1.1，附帶版權與編譯資訊，同時展示了ffmpeg關聯的程式庫檔案資訊，底部還標註了ffmpeg的使用說明，包括基本用法和取得更多說明的方式。該圖片用於驗證Windows電腦上ffmpeg的安裝是否成功，對應文件中「安裝ffmpeg」步驟後的驗證環節，能直觀呈現ffmpeg安裝完成後的執行狀態。](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![這是Linux終端機的操作介面，展示了conda相關操作的指令與執行過程。介面中明確標註了兩個核心指令，分別是啟用名為lerobot的虛擬環境的指令「$ conda activate lerobot」，以及關閉作用中環境的指令「$ conda deactivate」。目前處於(base)基礎環境已啟用的狀態，終端機正執行安裝ffmpeg 7.1.1版本的操作，指定從conda-forge渠道安裝，同步顯示了設定的多個軟體來源位址，同時能看到軟體套件中繼資料與相依環境的收集流程已完成。](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![圖片展示的是在Ubuntu系統下使用ffmpeg命令的終端機介面。顯示了ffmpeg版本資訊，包括版本號、編譯者、編譯器等設定詳情。還列出了多種編解碼器的版本，如libavcodec、libavformat等。底部有使用說明，提示使用「-h」取得全部說明，或執行「man ffmpeg」。該圖片與文件中「安裝ffmpeg」部分相關，用於驗證ffmpeg安裝成功，顯示其版本及編譯資訊。](../../en/images/d05-05.png)
</column>
</grid>

## 下載LeRobot

- 下載LeRobot官方程式庫

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## 安裝程式碼儲存庫

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![圖片展示的是在Windows系統cmd命令列中安裝LeRobot程式碼儲存庫後的驗證結果。命令列顯示「Successfully built lerobot」等資訊，表示安裝成功。還列出多個Python套件及其版本號，如numpy 1.22.3、scipy 1.7.1等。底部提示「(lerobot) C:\\Users\\40743\\Downloads\\lerobot>」，表示目前目錄為Downloads資料夾下的lerobot資料夾。該圖片與文件中「驗證安裝成功」部分對應，直觀呈現了安裝成功後的命令列回饋。](../../en/images/d05-06.png)

## 驗證安裝成功

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![圖片展示的是在Windows系統下Python環境驗證安裝成功的介面。命令列中顯示Python版本為3.10.19，執行了匯入lerobot、scservo_sdk、torch等模組的程式碼，最後執行torch.cuda.is_available()返回False。該圖片與文件中「驗證安裝成功」部分對應，直觀呈現了安裝LeRobot程式碼儲存庫後，透過Python環境驗證安裝是否成功的操作及結果。](../../en/images/d05-07.png)
