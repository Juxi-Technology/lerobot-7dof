[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | 繁體中文 | [Deutsch](../de/09-train-model/train-pi0fast.md) | [Español](../es/09-train-model/train-pi0fast.md) | [Français](../fr/09-train-model/train-pi0fast.md) | [Italiano](../it/09-train-model/train-pi0fast.md) | [日本語](../ja/09-train-model/train-pi0fast.md) | [한국어](../ko/09-train-model/train-pi0fast.md) | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# 訓練命令列-pi0fast

## 參考文件

https://huggingface.co/docs/lerobot/pi0fast

## Issue

https://github.com/huggingface/lerobot/pull/2203

## 推薦雲GPU實例

![圖片展示了推薦的雲GPU實例RTX A6000的相關資訊。顯示有3卡可用，按量使用價格為每小時3.29元。其設定為GPU RTX A6000，顯示記憶體共51.0GB，CPU是30核AMD EPYC 7742，記憶體60.9GB，硬碟429.5GB，底部還有「開始使用」按鈕。該圖片位於文件「推薦雲GPU實例」部分，為使用者提供訓練所需的雲GPU實例推薦及關鍵設定和價格等資訊。](../../en/images/d41-01.png)

## 安裝環境

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## 命令列

- 刪除之前訓練中斷的output下的檔案

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- 訓練

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi0_fast \
    --output_dir=output_lerobot_train/shake/pi0_fast_A \
    --job_name=shake_pi0_fast_A \
    --policy.pretrained_path=lerobot/pi0_fast_base \
    --policy.dtype=bfloat16 \
    --policy.gradient_checkpointing=true \
    --policy.chunk_size=10 \
    --policy.n_action_steps=10 \
    --policy.max_action_tokens=256 \
    --steps=50000 \
    --batch_size=8 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project
```





## 之前的內容

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0_fast \
  --output_dir=output_lerobot_train/shake/pi0_fast_A \
  --job_name=shake_pi0_fast_A \
  --policy.pretrained_path=lerobot/pi0_fast_base \
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









執行後10分鐘左右，訓練才會正式開始

模型壓縮檔5個G左右，解壓縮後7個G
