[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | 繁體中文 | [Deutsch](../de/10-inference/infer-smolvla.md) | [Español](../es/10-inference/infer-smolvla.md) | [Français](../fr/10-inference/infer-smolvla.md) | [Italiano](../it/10-inference/infer-smolvla.md) | [日本語](../ja/10-inference/infer-smolvla.md) | [한국어](../ko/10-inference/infer-smolvla.md) | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# 推論命令列-smolvla

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## Mac

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論命令列

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/smolvla/40K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=2000
```

<grid>
<column width-ratio="0.425772">
![圖片展示的是在Ubuntu系統下推論命令列介面。介面上方顯示了目前執行的命令，包括使用快取、使用Delta Joint Actions Aloha等參數設定。下方有多個關鍵資訊，如機器人id為「zihao_follower_arm」，最大相對目標為None，連接埠為「/dev/tty.usbmodemSAAF2193661」，以及關於VLM層數量減少至16的提示。該圖片與文件中Ubuntu推論命令列操作上下文相關，直觀呈現了命令列介面及部分關鍵參數設定情況。](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![圖片展示的是在Ubuntu系統下推論命令列操作的終端機介面。介面上方顯示了機器人相關設定資訊，如calibration_dir、cameras等參數。下方有多個json檔案的載入進度，如config.json、processor_config.json等，顯示載入百分比及大小。介面底部有日誌資訊，如「Mismatch between calibration values in the motor and the calibration file or no calibration file found」等，提示存在馬達標定值與標定檔案不匹配等問題。該圖片與文件中Ubuntu推論命令列操作上下文對應，展示了操作過程中的終端機回饋。](../../en/images/d50-02.png)
</column>
</grid>

## 效果

<grid><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
