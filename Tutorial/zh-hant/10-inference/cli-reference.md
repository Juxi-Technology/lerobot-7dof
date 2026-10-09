[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | 繁體中文 | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# 命令列說明

## 命令列說明

帶即時視覺化：--display_data=true

不帶即時視覺化：--display_data=false

`--display_data=true`時，會啟動rerun.io酷炫的視覺化介面，但在`/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000`目錄下，會儲存每一幀的圖片，但很佔空間。後續可以設定成`--display_data=false`



推論HuggingFace模型Repo的模型：--policy.path=Tommymy/lerobot_my_model_a



## 以抓橘子任務為例

- 推論本機模型（帶即時視覺化）

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

- 推論本機模型（不帶即時視覺化）

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

- 推論HuggingFace模型Repo的模型

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

執行後會下載模型

![圖片展示的是在命令列中執行`pretrained_model.py`腳本的介面。介面上方顯示了模型參數設定，如`--display_data=true`、`--policy.path=TommyZihao/lerobot_zihao_model_a`等。下方有「robot」「camera」「calibration_dir」等參數設定。最下方顯示了模型下載進度，目前下載了68%。該圖片與文件中介紹`pretrained_model.py`腳本執行時的模型參數設定及執行情況的內容相關，直觀呈現了腳本執行時的參數設定和下載進度情況。](../../en/images/d48-01.png)
