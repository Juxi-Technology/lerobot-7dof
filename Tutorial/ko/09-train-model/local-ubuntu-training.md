[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# 로컬 Ubuntu 학습

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- 주의

`\` 앞에는 공백이 한 칸만 있어야 하고, 뒤에는 공백이 없어야 합니다

`--dataset.split`의 기본값은 `train`이며, 이는 전체 데이터셋을 학습 세트로 사용한다는 뜻입니다

데이터셋이 로컬에 있으므로 `--dataset.streaming`은 반드시 `false`여야 합니다. 데이터가 이미 디스크에 있어 스트리밍 읽기가 필요 없기 때문입니다

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![이 이미지는 로컬 Ubuntu 환경에서 lerobot_train.py 스크립트를 실행하기 위한 학습 명령과 구성을 보여줍니다. 명령에는 데이터셋 경로, 저장소 ID와 브랜치 같은 파라미터가 포함되며, 예를 들어 `--dataset.repo_id`는 Tommy/lerobot_zhao_dataset_a로 설정되어 있습니다. 구성 값 중 `--dataset.streaming`은 false, `--use_imagenet_stats`는 True, `--batch_size`는 4, `--val_n_episodes`는 1000으로 되어 있습니다. 이 이미지는 문맥과 밀접하게 관련되어 학습 명령과 그 핵심 구성 파라미터를 시각적으로 보여줍니다.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![이 이미지는 로컬 Ubuntu 환경에서 lerobot_train.py 스크립트를 실행해 생성된 학습 로그로, 학습 구성 파라미터와 학습 실행의 실시간 상태를 중심으로 보여줍니다. 핵심 학습 정보가 명확히 표시되어 있습니다. `--dataset.split`의 기본값은 `train`이며 전체 데이터셋을 학습 세트로 사용한다는 뜻이고, 데이터셋이 로컬에 저장되어 있으므로 `--dataset.streaming`의 상태도 마찬가지로 고정됩니다. 로그에는 학습 진행 상황, 데이터셋 로딩, 모델 옵티마이저와 스케줄러 생성, 학습 중의 loss와 step 수치도 담겨 있어 로컬 학습이 진행 중인 모습을 분명하게 볼 수 있습니다.](../../en/images/d34-02.png)
</column>
</grid>

![이 이미지는 Ubuntu 환경에서 LeRobot로 모델을 학습할 때 생성된 학습 로그를 보여줍니다. 로그에는 시간, 학습 세트, 모델, loss, 정확도 등 학습 실행의 정보가 기록되어 있습니다. 타임스탬프는 2024년 1월 14일 15:11:53부터 16:15:16까지이며, loss는 0.68과 0.65 사이를 오가고 정확도(acc)는 0.85와 0.88 사이입니다. 이 이미지는 LeRobot 모델 학습 문맥과 관련되어, 학습 중 핵심 지표가 어떻게 변하는지 시각적으로 보여줍니다.](../../en/images/d34-03.png)
