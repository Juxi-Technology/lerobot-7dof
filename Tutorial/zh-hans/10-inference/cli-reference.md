[English](../en/10-inference/cli-reference.md) | 简体中文 | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# 命令行说明

## 命令行说明

带实时可视化：--display_data=true

不带实时可视化：--display_data=false

`--display_data=true`时，会启动rerun.io酷炫的可视化界面，但在`/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000`目录下，会保存每一帧的图片，但很占空间。后续可以设置成`--display_data=false`



推理HuggingFace模型Repo的模型：--policy.path=Tommymy/lerobot_my_model_a



## 以抓橘子任务为例

- 推理本地模型（带实时可视化）

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

- 推理本地模型（不带实时可视化）

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

- 推理HuggingFace模型Repo的模型

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

运行后会下载模型

![图片展示的是在命令行中运行`pretrained_model.py`脚本的界面。界面上方显示了模型参数配置，如`--display_data=true`、`--policy.path=TommyZihao/lerobot_zihao_model_a`等。下方有“robot”“camera”“calibration_dir”等参数设置。最下方显示了模型下载进度，当前下载了68%。该图片与文档中介绍`pretrained_model.py`脚本运行时的模型参数配置及运行情况的内容相关，直观呈现了脚本运行时的参数设置和下载进度情况。](../../en/images/d48-01.png)