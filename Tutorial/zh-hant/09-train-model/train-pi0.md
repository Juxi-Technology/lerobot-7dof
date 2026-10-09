[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | 繁體中文 | [Deutsch](../de/09-train-model/train-pi0.md) | [Español](../es/09-train-model/train-pi0.md) | [Français](../fr/09-train-model/train-pi0.md) | [Italiano](../it/09-train-model/train-pi0.md) | [日本語](../ja/09-train-model/train-pi0.md) | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# 訓練命令列-pi0（效果最好）

## 參考文件

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## 推薦雲GPU實例

![圖片展示的是RTX A6000雲GPU實例資訊。其按量使用價格為3.29元/小時，GPU為RTX A6000，共51.0GB顯示記憶體；CPU為30核AMD EPYC 7742；記憶體60.9GB；硬碟429.5GB。右上角顯示有3卡可用。底部有一個藍色的「開始使用」按鈕。該圖片與文件中「推薦雲GPU實例」部分內容相關，直觀呈現了推薦的雲GPU實例設定及價格等關鍵資訊。](../../en/images/d39-01.png)

## 安裝環境

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## 命令列

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_A

lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0 \
  --output_dir=~/output_lerobot_train/shake/pi0_A \
  --job_name=shake_pi0_A \
  --policy.pretrained_path=lerobot/pi0_base \
  --policy.compile_model=true \
  --policy.gradient_checkpointing=true \
  --policy.dtype=bfloat16 \
  --policy.freeze_vision_encoder=false \
  --policy.train_expert_only=false \
  --steps=50000 \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --batch_size=8
```

命令列執行20分鐘後，訓練才會正式開始

模型壓縮檔5個G左右，解壓縮後7個G
