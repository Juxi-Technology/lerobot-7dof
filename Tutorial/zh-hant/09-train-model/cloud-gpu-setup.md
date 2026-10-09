[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | 繁體中文 | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# 雲GPU訓練環境設定

## 關閉自己電腦的網路代理

不然可能打不開Jupyter的命令列

## 登入雲GPU平台Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

加使用者群，跟客服說是同濟子豪兄粉絲，領取代金券

## 開啟一個雲GPU執行個體

## 安裝設定環境

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## 登入wandb

```Shell
wandb login
复制粘贴API Key，回车
```

![圖片展示了在Lerobot環境下使用`wandb login`命令登入W&B（Weights & Biases）的過程。命令執行後，W&B提示登入到https://api.wandb.ai，可部署本機W&B伺服器，並引導尋找API金鑰。接著，W&B提示將API金鑰貼上並按Enter，還指出未找到.netrc檔案，正在建立。最後，W&B顯示已登入為tommyzihao，登入狀態為https://api.wandb.ai。該圖片與文件中「登入wandb」步驟相關，直觀呈現了登入操作的回饋資訊。](../../en/images/d35-01.png)

## 掛載資料集

```Shell
复制实例下载命令，类似：
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

資料集出現在`~`目錄下

## 修改權重儲存頻率（選做）

開啟`lerobot/src/lerobot/configs/train.py`

將save_freq，從20_000修改為5_000

這樣能在訓練更早期取得模型權重檔案
