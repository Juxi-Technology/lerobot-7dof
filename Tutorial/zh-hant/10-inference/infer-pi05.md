[English](../en/10-inference/infer-pi05.md) | [简体中文](../zh-hans/10-inference/infer-pi05.md) | 繁體中文 | [Deutsch](../de/10-inference/infer-pi05.md) | [Español](../es/10-inference/infer-pi05.md) | [Français](../fr/10-inference/infer-pi05.md) | [Italiano](../it/10-inference/infer-pi05.md) | [日本語](../ja/10-inference/infer-pi05.md) | [한국어](../ko/10-inference/infer-pi05.md) | [Português (BR)](../pt-br/10-inference/infer-pi05.md) | [Português (PT)](../pt-pt/10-inference/infer-pi05.md)

# 推論命令列-pi0.5

## Ubuntu

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
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
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi05/50K/pretrained_model
```










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
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![圖片展示的是在Ubuntu系統下使用Python 3.12及mujoco模擬環境進行機器人控制的命令列介面。介面中顯示了部分程式碼， 以及執行過程中出現的警告和錯誤資訊 addCriterion](../../en/images/d52-01.png)
</column>
<column width-ratio="0.534183">
![圖片展示的是在Ubuntu系統下使用Python 3.7.12和PyTorch 1.12.0版本執行相關程式碼時的輸出資訊。其中，有多個「WARNING」和「INFO」級別的警告和استف 自動生成](../../en/images/d52-02.png)
</column>
</grid>

## 推論很慢的原因

- 資料集太小了
- 顯示卡顯示記憶體不夠，需要上50系顯示卡
