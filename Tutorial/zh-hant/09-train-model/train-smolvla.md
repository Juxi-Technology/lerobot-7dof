[English](../en/09-train-model/train-smolvla.md) | [简体中文](../zh-hans/09-train-model/train-smolvla.md) | 繁體中文 | [Deutsch](../de/09-train-model/train-smolvla.md) | [Español](../es/09-train-model/train-smolvla.md) | [Français](../fr/09-train-model/train-smolvla.md) | [Italiano](../it/09-train-model/train-smolvla.md) | [日本語](../ja/09-train-model/train-smolvla.md) | [한국어](../ko/09-train-model/train-smolvla.md) | [Português (BR)](../pt-br/09-train-model/train-smolvla.md) | [Português (PT)](../pt-pt/09-train-model/train-smolvla.md)

# 訓練命令列-smolvla（推薦進階）

## 參考文件

https://huggingface.co/docs/lerobot/smolvla

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/policy_smolvla_README.md

## 安裝環境

```Shell
cd lerobot
pip install -e ".[feetech,smolvla]"
```

## 基於預訓練模型微調（推薦）

```Shell
lerobot-train \
  --policy.path=lerobot/smolvla_base \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=smolvla \
  --output_dir=~/output_lerobot_train/shake/smolvla_A \
  --job_name=shake_smolvla_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=40000 \
  --batch_size=8
```

## 從頭訓練

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=smolvla \
  --output_dir=~/output_lerobot_train/shake/smolvla_A \
  --job_name=shake_smolvla_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=40000 \
  --batch_size=8
```

## 下載模型

smolvla模型壓縮檔大概1個G左右
