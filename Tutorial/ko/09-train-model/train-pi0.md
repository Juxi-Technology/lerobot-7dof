[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | [Deutsch](../de/09-train-model/train-pi0.md) | [Español](../es/09-train-model/train-pi0.md) | [Français](../fr/09-train-model/train-pi0.md) | [Italiano](../it/09-train-model/train-pi0.md) | [日本語](../ja/09-train-model/train-pi0.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# 학습 명령줄 - pi0 (가장 좋은 결과)

## 참고 문서

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## 권장 클라우드 GPU 인스턴스

![이 이미지는 RTX A6000 클라우드 GPU 인스턴스의 상세 정보를 보여줍니다. 종량제 가격은 3.29 CNY/시간이고, GPU는 RTX A6000으로 GPU 메모리가 총 51.0 GB이며, CPU는 30코어 AMD EPYC 7742, 메모리는 60.9 GB, 디스크는 429.5 GB입니다. 오른쪽 상단에는 3장이 사용 가능하다고 표시되어 있습니다. 아래쪽에는 파란색 "Start Using" 버튼이 있습니다. 이 이미지는 "권장 클라우드 GPU 인스턴스" 절과 관련되어, 권장 클라우드 GPU 구성과 가격 등 핵심 정보를 시각적으로 보여줍니다.](../../en/images/d39-01.png)

## 환경 설치

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## 명령줄

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

명령줄이 20분 동안 실행된 뒤에야 학습이 제대로 시작됩니다

모델 아카이브는 약 5 GB이고, 압축을 풀면 약 7 GB입니다
