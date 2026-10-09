[English](../en/10-inference/infer-smolvla.md) | 简体中文 | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | [Deutsch](../de/10-inference/infer-smolvla.md) | [Español](../es/10-inference/infer-smolvla.md) | [Français](../fr/10-inference/infer-smolvla.md) | [Italiano](../it/10-inference/infer-smolvla.md) | [日本語](../ja/10-inference/infer-smolvla.md) | [한국어](../ko/10-inference/infer-smolvla.md) | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# 推理命令行-smolvla

## Ubuntu

- 删除原有的eval开头的数据集（如有）

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推理命令行

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

- 删除原有的eval开头的数据集（如有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推理命令行

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
![图片展示的是在Ubuntu系统下推理命令行界面。界面上方显示了当前运行的命令，包括使用缓存、使用Delta Joint Actions Aloha等参数设置。下方有多个关键信息，如机器人id为“zihao_follower_arm”，最大相对目标为None，端口为“/dev/tty.usbmodemSAAF2193661”，以及关于VLM层数量减少至16的提示。该图片与文档中Ubuntu推理命令行操作上下文相关，直观呈现了命令行界面及部分关键参数设置情况。](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![图片展示的是在Ubuntu系统下推理命令行操作的终端界面。界面上方显示了机器人相关配置信息，如calibration_dir、cameras等参数。下方有多个json文件的加载进度，如config.json、processor_config.json等，显示加载百分比及大小。界面底部有日志信息，如“Mismatch between calibration values in the motor and the calibration file or no calibration file found”等，提示存在电机校准值与校准文件不匹配等问题。该图片与文档中Ubuntu推理命令行操作上下文对应，展示了操作过程中的终端反馈。](../../en/images/d50-02.png)
</column>
</grid>

## 效果

<grid><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[附件 / Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>