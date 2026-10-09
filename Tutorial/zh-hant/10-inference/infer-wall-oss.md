[English](../en/10-inference/infer-wall-oss.md) | [简体中文](../zh-hans/10-inference/infer-wall-oss.md) | 繁體中文 | [Deutsch](../de/10-inference/infer-wall-oss.md) | [Español](../es/10-inference/infer-wall-oss.md) | [Français](../fr/10-inference/infer-wall-oss.md) | [Italiano](../it/10-inference/infer-wall-oss.md) | [日本語](../ja/10-inference/infer-wall-oss.md) | [한국어](../ko/10-inference/infer-wall-oss.md) | [Português (BR)](../pt-br/10-inference/infer-wall-oss.md) | [Português (PT)](../pt-pt/10-inference/infer-wall-oss.md)

# 推論命令列-WALL-OSS

## Ubuntu

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論命令列

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

![圖片展示的是在Ubuntu系統中推論命令列操作的終端機介面。介面上顯示了多個參數設定，如影片總像素數為90316800，前處理設定檔大小為2.46KB等。還呈現了tokenizer.json、tokenizer_config.json等檔案的載入進度，如tokenizer.json載入進度為100%。此外，介面上方有「INFO」提示資訊，下方有「Loading model from:」等載入模型相關提示。該圖片與文件中Ubuntu推論命令列操作上下文對應，直觀呈現了操作過程中的關鍵資訊。](../../en/images/d53-01.png)

<figure view-type="Preview">[附件 / Attachment: c7b8a795e52ca10689d296d212dc8e53.mp4](../../en/images/c7b8a795e52ca10689d296d212dc8e53.mp4)</figure>




## 推論命令列-Mac

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論命令列

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

<figure view-type="Preview">[附件 / Attachment: 9f4567abdcce7f983415aef8f42877d0.mp4](../../en/images/9f4567abdcce7f983415aef8f42877d0.mp4)</figure>
