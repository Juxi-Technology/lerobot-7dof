[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# 학습 명령줄 - pi0.5

## 참고 문서

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## 권장 클라우드 GPU 인스턴스

![이 이미지는 Alibaba Cloud에서 제공하는 RTX A6000 클라우드 GPU 인스턴스의 상세 정보를 보여줍니다. 종량제 가격은 3.29 CNY/시간이고 3장이 사용 가능합니다. 인스턴스는 GPU 메모리가 총 51.0 GB인 RTX A6000 GPU, 30코어 AMD EPYC 7742 CPU, 60.9 GB 메모리, 429.5 GB 디스크로 구성되어 있습니다. 아래쪽에는 파란색 "Start Using" 버튼이 있습니다. 이 이미지는 "권장 클라우드 GPU 인스턴스" 절과 관련되어, 권장 클라우드 GPU 구성과 가격 등 핵심 정보를 시각적으로 보여줍니다.](../../en/images/d40-01.png)

## 환경 설치

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## 명령줄

- 이전에 중단된 학습 실행이 남긴 output 아래의 파일을 삭제합니다

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- 학습

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi05 \
    --output_dir=~/output_lerobot_train/shake/pi05_A \
    --job_name=shake_pi05_A \
    --policy.pretrained_path=lerobot/pi05_base \
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

<grid>
<column width-ratio="0.468128">
![이 이미지는 명령줄에서 물리 지능(PI) 모델을 학습할 때의 출력을 보여줍니다. 모델이 로드되고, 파라미터가 재매핑되며, 옵티마이저와 스케줄러가 생성되는 과정을 보여주며, 예를 들어 "Loading model from: lerobot/pi05_base"가 있습니다. "Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy" 같은 경고 메시지도 나타납니다. 또한 "num_total_frames: 180K" 같은 학습 관련 수치도 제시합니다. 이 이미지는 학습 명령줄 작업과 관련되어, 학습 실행의 핵심 정보를 시각적으로 보여줍니다.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![이 이미지는 명령줄 학습 중의 출력을 보여줍니다. 학습 중에 huggingface/torch 프로세스가 포크되는데, 이미 병렬 처리가 사용 중이므로 교착 상태를 피하기 위해 병렬 처리가 비활성화되며, "가능하면 포크 전에" 이렇게 하지 말 것을 권고하는 메시지가 표시됩니다. 학습은 20분이 지나야 제대로 시작됩니다. 이 이미지는 문맥과 밀접하게 관련되어, 학습 중 나타날 수 있는 프로세스 작업과 권고 메시지를 제시하고 학습의 상태와 주의할 점을 이해하는 데 도움을 줍니다.](../../en/images/d40-02.png)
</column>
</grid>

명령줄이 20분 동안 실행된 뒤에야 학습이 제대로 시작됩니다

모델 아카이브는 약 5 GB이고, 압축을 풀면 약 7 GB입니다
