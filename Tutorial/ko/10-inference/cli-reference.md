[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | 한국어 | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# 명령줄 참조

## 명령줄 주의사항

실시간 시각화 사용: --display_data=true

실시간 시각화 미사용: --display_data=false

`--display_data=true`로 설정하면 멋진 rerun.io 시각화 인터페이스가 실행되지만, `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` 디렉터리에 프레임마다 이미지가 저장되어 공간을 많이 차지합니다. 나중에 `--display_data=false`로 설정해도 됩니다.



HuggingFace 모델 저장소에서 모델 추론: --policy.path=Tommymy/lerobot_my_model_a



## 오렌지 집기(Grab Oranges) 작업을 예로 들면

- 로컬 모델 추론(실시간 시각화 사용)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- 로컬 모델 추론(실시간 시각화 미사용)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- HuggingFace 모델 저장소에서 모델 추론

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --policy.path=Tommymy/lerobot_my_model_a
```

실행 후 모델이 다운로드됩니다

![이 이미지는 명령줄에서 `pretrained_model.py` 스크립트를 실행하는 인터페이스를 보여줍니다. 상단에는 `--display_data=true`, `--policy.path=TommyZihao/lerobot_zihao_model_a` 같은 모델 파라미터 구성이 표시되어 있습니다. 아래에는 "robot", "camera", "calibration_dir" 등의 파라미터 설정이 있습니다. 하단에는 현재 68%인 모델 다운로드 진행률이 표시되어 있습니다. 이 이미지는 `pretrained_model.py` 스크립트 실행과 그 파라미터 설명과 관련되어, 파라미터 설정과 다운로드 진행률을 시각적으로 제시합니다.](../../en/images/d48-01.png)
