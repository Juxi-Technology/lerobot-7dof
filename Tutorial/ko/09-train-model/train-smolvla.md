[English](../en/09-train-model/train-smolvla.md) | [简体中文](../zh-hans/09-train-model/train-smolvla.md) | [繁體中文](../zh-hant/09-train-model/train-smolvla.md) | [Deutsch](../de/09-train-model/train-smolvla.md) | [Español](../es/09-train-model/train-smolvla.md) | [Français](../fr/09-train-model/train-smolvla.md) | [Italiano](../it/09-train-model/train-smolvla.md) | [日本語](../ja/09-train-model/train-smolvla.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/train-smolvla.md) | [Português (PT)](../pt-pt/09-train-model/train-smolvla.md)

# 학습 명령줄 - smolvla (다음 단계로 권장)

## 참고 문서

https://huggingface.co/docs/lerobot/smolvla

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/policy_smolvla_README.md

## 환경 설치

```Shell
cd lerobot
pip install -e ".[feetech,smolvla]"
```

## 사전학습 모델에서 파인튜닝(권장)

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

## 처음부터 학습

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

## 모델 다운로드

smolvla 모델 아카이브는 약 1 GB입니다
