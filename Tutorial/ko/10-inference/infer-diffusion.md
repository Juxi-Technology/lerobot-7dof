[English](../en/10-inference/infer-diffusion.md) | [简体中文](../zh-hans/10-inference/infer-diffusion.md) | [繁體中文](../zh-hant/10-inference/infer-diffusion.md) | [Deutsch](../de/10-inference/infer-diffusion.md) | [Español](../es/10-inference/infer-diffusion.md) | [Français](../fr/10-inference/infer-diffusion.md) | [Italiano](../it/10-inference/infer-diffusion.md) | [日本語](../ja/10-inference/infer-diffusion.md) | 한국어 | [Português (BR)](../pt-br/10-inference/infer-diffusion.md) | [Português (PT)](../pt-pt/10-inference/infer-diffusion.md)

# 추론 명령줄 - Diffusion

## Ubuntu

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 추론 명령줄

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/Diffusion/15K/pretrained_model
```

## Mac

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 추론 명령줄

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands

lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/Diffusion/5K/pretrained_model
```

## 결과: 눈에 띄는 끊김

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: ea9ffc82e6501aa1c3af9d74dc551650.mp4](../../en/images/ea9ffc82e6501aa1c3af9d74dc551650.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: 02cfb7022f0828428e6fb10d62dc0e77.mp4](../../en/images/02cfb7022f0828428e6fb10d62dc0e77.mp4)</figure></column></grid>
