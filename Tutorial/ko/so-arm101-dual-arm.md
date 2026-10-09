[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | 한국어 | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# SO-ARM101 듀얼 암 튜토리얼

## 소개

이 가이드는 LeRobot으로 듀얼 암 SO-ARM 로봇 시스템을 학습시키는 전체 워크플로를 다룹니다. 하드웨어 배선, 듀얼 암 캘리브레이션, 듀얼 암 원격조작, 데이터셋 기록과 관리, ACT 정책 학습, 실제 로봇 배포를 포함합니다. 이 가이드를 따르면 두 개의 Leader 암과 두 개의 Follower 암으로 시연 데이터를 수집하고, 모방학습 정책을 학습시키고, 실제 암에서 실행할 수 있습니다.

먼저 다음과 같이 배선합니다

| 역할 | 포트 |
|-|-|
| 왼쪽 Follower | /dev/ttyACM0 |
| 오른쪽 Follower | /dev/ttyACM1 |
| 왼쪽 Leader | /dev/ttyACM2 |
| 오른쪽 Leader | /dev/ttyACM3 |

Follower 유형은 so101_follower이고 Leader 유형은 so101_leader입니다 (LeRobot에서 so100_leader와 so101_leader는 동일한 구현을 공유합니다).

## 사전 요구 사항

### 0.1 의존성 설치

환경 설정은 SO-ARM 튜토리얼을 참고하십시오:

### 0.2 USB 권한

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## 캘리브레이션 (중요 단계)

### 1.1 왼쪽 Follower 캘리브레이션

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 오른쪽 Follower 캘리브레이션

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 왼쪽 Leader 캘리브레이션

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 오른쪽 Leader 캘리브레이션

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

캘리브레이션 후 파일은 다음 위치에 저장됩니다:

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> 디렉터리 이름 참고: so101_follower와 so100_follower, 그리고 so101_leader와 so100_leader는 동일한 구현을 공유하므로 디렉터리는 so_follower / so_leader로 통일됩니다. Leader는 텔레오퍼레이터이므로 캘리브레이션 파일이 robots/가 아니라 teleoperators/ 아래에 위치합니다.

### (선택 사항) 이전에 다른 ID로 캘리브레이션한 경우

예를 들어 이전에 my_awesome_follower_arm1, my_awesome_follower_arm2 등을 사용했다면 캘리브레이션 파일을 복사할 수 있습니다:

```Bash
CAL_DIR=~/.cache/huggingface/lerobot/calibration

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm1.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_left.json

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm2.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_right.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm3.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_left.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm4.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_right.json
```

---

## 듀얼 암 원격조작

### 2.1 카메라 없이

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### 2.2 카메라 사용

lerobot-find-cameras opencv로 카메라 인덱스를 확인할 수 있으며, 카메라는 원하는 대로 추가하거나 제거할 수 있습니다.

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### 안전 수칙

- 주변을 주시하고 Follower 암끼리 충돌하지 않도록 하십시오.

## 데이터셋 기록

### 3.1 로컬 저장 (Hub에 업로드하지 않음)

--dataset.root (데이터가 기록되는 디렉터리)와 --dataset.push_to_hub=false를 추가하고, --dataset.no_stamp=true를 추가해 데이터셋 이름을 안정적으로 유지하십시오 (그렇지 않으면 repo_id에 타임스탬프가 자동으로 덧붙어 이후 재개/재생/학습 시 찾지 못합니다).

> 참고: repo_id에는 /가 포함되어야 합니다 (username/dataset-name 형식). 로컬 데이터셋은 실제로 업로드되지 않습니다.

```Bash
lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> 비디오 인코딩은 기본적으로 이미 libsvtav1이므로 별도로 지정할 필요가 없습니다. 커스터마이징하려면 --dataset.rgb_encoder.vcodec=h264와 같은 중첩 파라미터를 사용하십시오.

데이터는 ./datasets/bi_so101_task/ 아래에 다음 구조로 저장됩니다:

```Bash
├── meta/
│   ├── info.json         # Dataset info (fps, feature shapes, etc.)
│   ├── episodes/         # Per-episode metadata (chunk-000/...)
│   ├── stats.json        # Normalization stats for each feature
│   └── tasks.parquet     # Task text → task_index
├── data/                 # Per-frame feature data (chunk-*.parquet)
└── videos/               # One subdirectory per camera (chunk-*.mp4)
```

### 3.2 Hugging Face Hub에 업로드

자동 업로드를 원하면 HF_USER를 유지하고 root와 push_to_hub=false를 제거하십시오 (업로드가 기본값입니다). 포트와 카메라 인덱스는 배선 표와 일치하도록 유지하십시오:

```Bash
export HF_USER=your_hf_username

lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> 업로드되는 Hub 저장소 이름은 ${HF_USER}/bi_so101_task이며, 아래 4.2의 Hub 기반 학습에 사용되는 repo_id와 일치합니다. 로컬 복사본은 먼저 ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/에 저장됩니다.

### 3.3 기록 계속하기 (resume)

기록이 예기치 않게 종료되었거나 (예를 들어 리셋 단계에서 우클릭으로 종료한 경우), 여러 세션에 걸쳐 수집을 완료하려면 --resume을 사용해 동일한 데이터셋에 에피소드를 계속 추가하십시오.

**참고**:

- --resume=true를 반드시 추가해야 합니다. 그렇지 않으면 디렉터리가 이미 존재한다는 이유로 LeRobotDataset.create()가 오류를 냅니다.
- 재개 명령에서 --dataset.root와 --dataset.repo_id는 첫 기록 (3.1)과 정확히 일치해야 합니다 (재개에는 명시적 root가 필요합니다).
- --dataset.num_episodes는 **이번에 기록할 에피소드 수**이며 총 목표가 아닙니다. 예를 들어 이미 15개를 기록했고 총 50개를 원하면 35를 입력하십시오.
- 종료할 때는 에피소드 기록 중이나 자연스럽게 끝난 직후에 종료하십시오. "Reset the environment" 단계 중에는 종료하지 마십시오 (빈 에피소드가 저장되지 않고 실패합니다).

```Bash
lerobot-record \
  --resume=true \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=35 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

### 3.4 재생 및 에피소드 삭제

#### 특정 에피소드 재생

```Bash
lerobot-replay \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.episode=24
```

> episode는 0 기반 인덱스이므로 24는 25번째 에피소드를 의미합니다.

#### 특정 에피소드 삭제

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

삭제하면 데이터셋이 제자리에서 다시 작성되며, 원본 데이터는 ./datasets/bi_so101_task_old/에 백업됩니다. 새 데이터셋이 올바른지 확인한 뒤 백업을 수동으로 제거할 수 있습니다:

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### 데이터셋 전체 삭제

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## ACT 학습

### 4.1 로컬 데이터셋에서 학습

```Bash
lerobot-train \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=60000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> --dataset.root는 3.1에서 기록한 데이터셋 디렉터리를 가리킵니다 (repo_id는 기록 시 사용한 것과 일치해야 합니다). --output_dir 디렉터리가 이미 존재하면 즉시 FileExistsError가 발생합니다. 새 출력 디렉터리를 사용하거나 --resume=true를 추가해 학습을 계속하십시오.

### 4.2 Hugging Face Hub에서 학습

```Bash
export HF_USER=your_hf_username

lerobot-train \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=100000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> 위 명령은 ACT의 기본 파라미터를 사용합니다 (chunk_size=100, dim_model=512 등).
> 
> repo_id는 3.2에서 업로드할 때 사용한 저장소 이름과 일치해야 합니다 (3.2는 --dataset.no_stamp=true를 추가하므로 저장소 이름은 \${HF_USER}/bi_so101_task로 고정됩니다). 학습에는 --dataset.root가 필요하지 않으며, Hub에서 자동으로 다운로드됩니다.

## 실제 로봇 배포

> 참고: lerobot-record는 시연 데이터 수집 전용입니다. 학습된 정책을 배포하려면 lerobot-rollout을 사용하십시오. 현재 버전의 lerobot-record는 --policy.path를 더 이상 허용하지 않고, eval\_ 접두사가 붙은 데이터셋 이름도 거부합니다.

### 5.1 현장 평가 (데이터 기록 없음)

```Bash
lerobot-rollout \
  --strategy.type=base \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --task="Pick the cube with left arm and hand it to right arm" \
  --duration=60 \
  --display_data=true
```

- --duration은 실행할 초 수이며, 0은 시간 제한 없음을 의미합니다.
- 실행 중 개입/중지하려면 --interactive=true를 추가하고 터미널에서 /stop, /reset 등의 명령을 사용하십시오.

### 5.2 평가 및 데이터 기록 (로컬)

episodic 전략을 사용하십시오 (이전 lerobot-record처럼 리셋 단계를 두고 에피소드 단위로 기록합니다):

```Bash
lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=juxi/rollout_bi_so101_task \
  --dataset.root=./datasets/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

> 배포 데이터셋 이름은 rollout\_으로 시작해야 합니다 (현재 버전의 필수 요구 사항입니다). 로컬에 기록할 때는 --dataset.root와 --dataset.no_stamp=true를 추가해 디렉터리 이름에 타임스탬프가 붙지 않도록 하십시오.

### 5.3 평가 데이터를 Hugging Face Hub에 업로드

```Bash
export HF_USER=your_hf_username

lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=${HF_USER}/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

## FAQ

| 문제 | 원인 | 해결 방법 |
|-|-|-|
| 원격조작 시 재캘리브레이션을 요구함 | bi_so_follower가 \_left / \_right 접미사가 붙은 캘리브레이션 파일을 찾지 못함 | _left / \_right가 포함된 ID로 재캘리브레이션하거나 기존 캘리브레이션 파일을 복사합니다 |
| Leader 암을 끌 수 없음 | Leader 토크가 비활성화되지 않음 | 재캘리브레이션하거나 모터를 점검합니다 |
| 기록 재개 시 디렉터리가 이미 존재한다고 보고됨 | --resume=true를 추가하지 않음 | lerobot-record 명령에 --resume=true를 추가합니다 |
| --resume=true 실행 시 오류가 나며 root를 요구함 | 재개에는 명시적 데이터셋 디렉터리가 필요함 | 재개 명령에 첫 기록과 일치하는 --dataset.root=./datasets/bi_so101_task를 추가합니다 |
| 데이터셋 디렉터리 이름에 타임스탬프가 추가되어 재생/학습이 찾지 못함 | 기록 시 no_stamp를 설정하지 않아 repo_id에 타임스탬프가 붙음 | 기록/재개 시 --dataset.no_stamp=true를 추가합니다 |
| --dataset.vcodec=... 가 해당 파라미터가 없다고 보고함 | 오래된 파라미터이며, 비디오 인코딩 파라미터가 이제 중첩됨 | --dataset.rgb_encoder.vcodec=h264를 대신 사용합니다 (기본값은 이미 libsvtav1) |
| 배포 중 lerobot-record가 --policy.path / eval\_ 오류를 보고함 | 현재 버전의 lerobot-record에는 정책 배포가 더 이상 포함되지 않음 | 배포에는 lerobot-rollout --strategy.type=episodic을 사용하고, 데이터셋 이름은 rollout_으로 시작합니다 |
| 왼쪽과 오른쪽 팔이 뒤바뀜 | 포트 설정이 잘못됨 | left_arm_config.port와 right_arm_config.port를 교체합니다 |
| 학습이 데이터셋을 찾지 못함 | 로컬 데이터셋에 root를 지정하지 않음 | 학습 시 --dataset.root=./datasets/xxx를 추가합니다 |
| 데이터셋이 자동 업로드됨 | push_to_hub=false를 설정하지 않음 | 기록 시 --dataset.push_to_hub=false를 추가합니다 |
| 종료 시 You must add one or several frames before calling add_episode가 보고됨 | 리셋 단계 중에 종료하여 현재 에피소드에 프레임이 없음 | 이미 기록된 데이터에는 영향이 없으며, --resume=true로 수집을 계속합니다 |
