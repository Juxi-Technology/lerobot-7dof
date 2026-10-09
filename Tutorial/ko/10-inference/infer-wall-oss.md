[English](../en/10-inference/infer-wall-oss.md) | [简体中文](../zh-hans/10-inference/infer-wall-oss.md) | [繁體中文](../zh-hant/10-inference/infer-wall-oss.md) | [Deutsch](../de/10-inference/infer-wall-oss.md) | [Español](../es/10-inference/infer-wall-oss.md) | [Français](../fr/10-inference/infer-wall-oss.md) | [Italiano](../it/10-inference/infer-wall-oss.md) | [日本語](../ja/10-inference/infer-wall-oss.md) | 한국어 | [Português (BR)](../pt-br/10-inference/infer-wall-oss.md) | [Português (PT)](../pt-pt/10-inference/infer-wall-oss.md)

# 추론 명령줄 - WALL-OSS

## Ubuntu

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 추론 명령줄

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

![이 이미지는 Ubuntu 환경에서의 추론 명령줄 세션 중 터미널을 보여줍니다. 총 비디오 픽셀 수 90316800, 전처리 구성 파일 크기 2.46 KB 등 여러 파라미터 설정이 표시되어 있습니다. 또한 tokenizer.json, tokenizer_config.json 등 파일의 로딩 진행률, 예를 들어 tokenizer.json이 100% 로딩된 것을 보여줍니다. 상단에는 "INFO" 메시지가 있고, 아래에는 "Loading model from:" 같은 모델 로딩 안내가 있습니다. 이 이미지는 Ubuntu 추론 명령줄에 대응해, 실행 중의 핵심 정보를 제시합니다.](../../en/images/d53-01.png)

<figure view-type="Preview">[Attachment: c7b8a795e52ca10689d296d212dc8e53.mp4](../../en/images/c7b8a795e52ca10689d296d212dc8e53.mp4)</figure>




## 추론 명령줄 - Mac

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 추론 명령줄

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

<figure view-type="Preview">[Attachment: 9f4567abdcce7f983415aef8f42877d0.mp4](../../en/images/9f4567abdcce7f983415aef8f42877d0.mp4)</figure>
