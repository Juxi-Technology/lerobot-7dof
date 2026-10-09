[English](../en/10-inference/infer-act.md) | [简体中文](../zh-hans/10-inference/infer-act.md) | 繁體中文 | [Deutsch](../de/10-inference/infer-act.md) | [Español](../es/10-inference/infer-act.md) | [Français](../fr/10-inference/infer-act.md) | [Italiano](../it/10-inference/infer-act.md) | [日本語](../ja/10-inference/infer-act.md) | [한국어](../ko/10-inference/infer-act.md) | [Português (BR)](../pt-br/10-inference/infer-act.md) | [Português (PT)](../pt-pt/10-inference/infer-act.md)

# 推論命令列-ACT

## Ubuntu

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論命令列

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/ACT/5K/pretrained_model
```

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768807937129.mp4](../../en/images/wx_camera_1768807937129.mp4)</figure>

## Mac

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論命令列

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/ACT/5K/pretrained_model
```

## 效果

雖然還有點抖，但是已經是個不錯的開始了

<grid><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: cd2d441433b756e54b1a75359e99f7a4.mp4](../../en/images/cd2d441433b756e54b1a75359e99f7a4.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: 7fe9ac43b5d9d531f7bd3b715615eb31.mp4](../../en/images/7fe9ac43b5d9d531f7bd3b715615eb31.mp4)</figure></column></grid>
