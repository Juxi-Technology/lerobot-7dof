[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | 简体中文 | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# 示教采集数据集-握手200

## 在HuggingFace上创建Dataset Repo

https://huggingface.co/new-dataset

![图片展示的是在HuggingFace上创建新数据集仓库的界面。界面中“Owner”处显示为TommyZihao，“Dataset name”输入框内填写为lerobot_zihao_dataset_shake200，“License”选择为mit，“Public”选项被选中，下方说明任何人都可查看此数据集，只有数据集所有者或组织成员可提交。底部提示数据集创建后，可使用网页界面或git上传文件，且有“Create dataset”按钮。该图片与文档中在HuggingFace上创建Dataset Repo的内容相关，是创建数据集仓库操作的界面展示。](../../en/images/d27-01.png)

## 删除之前已经有的同名数据集（如果有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Shake200数据集采集

一个摄像头，采集数据集-Mac电脑

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
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake200 \
    --dataset.num_episodes=200 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```

## 采集中

<grid>
<column width-ratio="0.508765">
![这是一段用于采集握手数据集（Shake200）时录屏终端的界面内容，界面顶部和中部偏下均有标注为“INFO 202-04-15 17:00:22.227 Recording episode 36”的信息，显示正在录制第36个数据采集周期（episode），同时呈现了SVT-AV1编码器的相关参数，以及进度信息“200/200 \[00:00:00::00, 4911.09 examples/s\]”，体现该阶段正处于数据集采集过程中，与文档描述的“采集中”环节对应，展示了采集时的系统运行状态。](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![这张图片是Mac终端的运行界面，显示了SVT项目的配置与运行日志信息。界面中包含多项配置参数，如SVT配置的 GOP 大小、mini-pod 大小、关键帧类型、CRF 等数值，还有SVT-A1 Encoder库的版本信息。界面下半部分的日志记录了视频文件操作相关的内容，提到移动原子到文件开头，以及多个OpenVCamera、SV280 follower等组件的连接与断开状态，最终显示当前路径下的终端执行结束提示，对应示教采集握手200数据集过程中的终端运行场景。](../../en/images/d27-02.png)
</column>
</grid>

键盘方向键操作：  
→（右箭头）提前终止当前episode；进入下一个episode。  
←（左箭头）取消当前episode；重新录制。  
ESC，立即停止，编码视频，并上传数据集。

## 采集完毕，数据集保存目录

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
