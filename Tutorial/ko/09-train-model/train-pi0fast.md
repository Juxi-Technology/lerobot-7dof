[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | [Deutsch](../de/09-train-model/train-pi0fast.md) | [Español](../es/09-train-model/train-pi0fast.md) | [Français](../fr/09-train-model/train-pi0fast.md) | [Italiano](../it/09-train-model/train-pi0fast.md) | [日本語](../ja/09-train-model/train-pi0fast.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# 학습 명령줄 - pi0fast

## 참고 문서

https://huggingface.co/docs/lerobot/pi0fast

## 이슈

https://github.com/huggingface/lerobot/pull/2203

## 권장 클라우드 GPU 인스턴스

![이 이미지는 권장 RTX A6000 클라우드 GPU 인스턴스의 상세 정보를 보여줍니다. 3장이 사용 가능하고 종량제 가격이 3.29 CNY/시간임을 나타냅니다. 구성은 GPU 메모리가 총 51.0 GB인 RTX A6000 GPU, 30코어 AMD EPYC 7742 CPU, 60.9 GB 메모리, 429.5 GB 디스크이며, 아래쪽에 "Start Using" 버튼이 있습니다. 이 이미지는 "권장 클라우드 GPU 인스턴스" 절에 위치하며, 사용자에게 학습용 클라우드 GPU 추천과 그 핵심 구성 및 가격을 제공합니다.](../../en/images/d41-01.png)

## 환경 설치

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## 명령줄

- 이전에 중단된 학습 실행이 남긴 output 아래의 파일을 삭제합니다

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- 학습

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




## 이전 내용

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








실행 후 약 10분이 지나야 학습이 제대로 시작됩니다

모델 아카이브는 약 5 GB이고, 압축을 풀면 약 7 GB입니다
