[English](../en/07-collect-dataset-real/collect-dataset.md) | 简体中文 | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# 示教采集数据集

## 删除之前已经有的同名数据集（如果有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## 一个摄像头，采集数据集-Mac电脑

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## 两个摄像头，采集数据集-Mac电脑

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=true \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## 采集中

<grid>
<column width-ratio="0.508765">
![图片展示的是在Mac电脑上使用OpenVSLAM采集数据集时的终端界面。界面上方显示了采集参数，如分辨率、帧率、编码器等信息。下方是采集日志，记录了采集开始时间、版本信息、线程数、编码器等数据，还显示了采集进度，如已采集298/298个episode，共5119.33秒。界面底部有“ESC”键操作说明，如立即停止并上传数据集。该图片与文档中采集数据集的操作流程相关，直观呈现了采集过程中的终端反馈信息。](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![这张图片展示了Mac电脑的命令行终端界面，用于显示摄像头采集数据集相关的运行日志信息。界面中包含SVT相关配置参数，如config参数、编码库版本信息、各配置项的数值（如key frame、CRF、编码分辨率等），还能看到运行过程中的状态日志，例如MP4文件处理的相关提示、设备断开连接的记录、程序运行时的时间戳信息等，整体呈现了摄像头采集数据集过程中的后台运行状态。](../../en/images/d26-02.png)
</column>
</grid>

键盘方向键操作：  
→（右箭头）提前终止当前episode；进入下一个episode。  
←（左箭头）取消当前episode；重新录制。  
ESC，立即停止，编码视频，并上传数据集。

## 采集完毕，数据集保存目录

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```





## 握手

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.num_episodes=30 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```