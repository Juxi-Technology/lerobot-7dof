[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# 학습 명령줄 - ACT (초보자에게 권장)

## 참고 문서

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## 왜 ACT 알고리즘부터 시작하는가

ACT는 LeRobot에 입문할 때 가장 먼저 학습해 보도록 권장하는 모델입니다. 장점은 다음과 같습니다.

- 모델이 매우 가벼워 학습 가능한 파라미터가 8천만 개(80 million)뿐입니다
- 학습 수렴이 빠르고 추론도 빠릅니다
- GPU 한 장에서 한 시간만 학습해도 결과를 볼 수 있습니다
- ACT 모델 아카이브는 약 200 MB로, 보관과 전송이 쉽습니다
- 데이터를 약 30 에피소드 정도 수집하면 보통 충분합니다
- Ubuntu 호스트, Mac, Windows PC는 물론 라즈베리 파이에서도 추론 배포가 가능합니다
- 실제 로봇에서의 추론 성능이 꽤 좋아, 집기, 악수, 펜 놓기 같은 단순 작업에는 충분하고도 남습니다
- ACT 알고리즘은 이미 LeRobot 기본 환경에 내장되어 있어 별도 라이브러리가 필요 없습니다

## 명령줄

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## 명령줄 주의사항

줄 연결 기호 `\` 앞에는 공백이 한 칸만 있어야 하고, 뒤에는 공백이 없어야 합니다

빨간색으로 표시된 파라미터는 매번 실행 전에 확인하거나 변경해야 합니다

| 명령줄 파라미터 | 설명 |
|-|-|
| --dataset.repo_id | HuggingFace 데이터셋의 Repo_ID |
| --dataset.root | 데이터셋의 로컬 경로 |
| --dataset.revision | 데이터셋 버전. HuggingFace에 데이터셋을 업로드할 때 지정한 값입니다 |
| --dataset.streaming | 데이터셋이 로컬에 있으므로 반드시 `false`여야 합니다. 데이터가 이미 디스크에 있어 스트리밍 읽기가 필요 없기 때문입니다 |
| --dataset.split | 기본값은 `train`이며, 전체 데이터셋을 학습 세트로 사용한다는 뜻입니다 |
| --policy.type | 학습할 알고리즘. act, smolvla, diffusion, pi0, wallx 등 |
| --output_dir | 출력 구조가 저장되는 디렉터리 |
| --job_name | 이 학습 작업의 이름 |
| --policy.device | 연산 장치 |
| --wandb.enable | wandb 시각화 활성화 여부 |
| --wandb.project | wandb 프로젝트 이름 |
| --policy.push_to_hub | 학습한 모델을 HuggingFace로 푸시 |
| --steps | 학습 step 수 |
| --batch_size | 매 step에 공급하는 데이터 양. GPU 메모리가 부족하면 낮추세요 |
|  |  |

## 학습 과정

<grid>
<column width-ratio="0.357753">
![이 이미지는 명령줄에서 `lerobot-train` 학습 명령을 사용하는 예를 보여줍니다. 명령은 `--dataset.repo_id`, `--dataset.root` 등 여러 파라미터를 설정해 데이터셋 정보를 지정하고, `--policy.type`을 `act`, `--output_dir`을 출력 디렉터리 `outputs/lerobot_train/output_a`로 설정하며, `--job_name`, `--policy.device` 등 다른 파라미터도 함께 지정합니다. 또한 `--dataset.split`, `--policy.push_to_hub` 같은 파라미터의 기본값도 나열합니다. 이 이미지는 문맥과 밀접하게 관련되어, 학습 명령의 파라미터가 어떻게 설정되는지 시각적으로 보여줍니다.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![이 이미지는 학습 중 명령줄 출력을 보여줍니다. scheduler, steps, use_policy_training_preset 등의 설정과 데이터셋 관련 파라미터 같은 모델 학습 세부 정보를 표시합니다. 또한 모델 파라미터 개수와 loss 같은 정보, 예를 들어 num_total_params가 55917096(52M)이고 loss가 0.626인 것을 보여줍니다. 그 아래에는 "https://download.pytorch.org/models/resnet18-f37072fd.pth"를 /home/featurize/.cache/torch/hub/checkpoints 디렉터리로 다운로드하는 파일 다운로드 정보가 있습니다. 이 이미지는 문맥에서 설명한 학습 명령줄과 관련되어, 학습 중 명령줄이 출력하는 내용을 시각적으로 보여줍니다.](../../en/images/d36-02.png)
</column>
</grid>

![이 이미지는 학습 중 생성된 로그 정보를 보여줍니다. 로그에는 시간 순서대로 여러 학습 step이 기록되어 있으며, 시간, 학습 반복 횟수, loss, 학습률이 포함됩니다. 예를 들어 2024년 1월 14일 15:11:53에 반복 횟수는 131k, loss는 0.368이었습니다. 여기서 `INFO`는 로그 유형, `train`은 학습 단계, `step`은 반복 횟수, `loss`는 손실값, `lr`은 학습률입니다. 이 이미지는 문서의 명령줄 주의사항 절과 관련되어, 학습 실행의 핵심 데이터를 시각적으로 보여줍니다.](../../en/images/d36-03.png)

모델 아카이브는 약 300 MB입니다
