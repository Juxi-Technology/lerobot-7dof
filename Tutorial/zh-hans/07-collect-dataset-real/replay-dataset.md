[English](../en/07-collect-dataset-real/replay-dataset.md) | 简体中文 | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# 回看、回放数据集

## 可视化整个数据集

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

输入`TommyZihao/lerobot_zihao_dataset_a`，或者其它数据集

![图片展示的是LeRobot Dataset Visualizer界面，画面中有一个机器人，界面上方有“LeRobot Dataset Visualizer”字样。界面中部有一个下拉菜单，显示“TommyZihao/lerobot_zihao_dataset_a”等数据集选项，还有“Example Datasets”及其下的具体数据集名称，下方有“Explore Open Datasets”蓝色按钮。该图片与上文提到的可视化整个数据集相关，对应输入指定数据集的操作内容。](../../en/images/d29-01.png)

![图片展示 addCriterion addCriterion](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![图片展示的是LeroBot抓取橙橘子数据集的可视化界面。画面中上方是抓取橙子的视频，橙子被夹在白色物体上。下方是数据图表，显示了多个变量随时间变化的曲线，如“actuator”“gripper”“gripper_pos”等。左侧有指令列表，当前选中“Grab Orangesanges”。右下角有播放、暂停等操作按钮。该图与上文提到的可视化查看指定episode内容相关，直观呈现了抓取动作及对应数据。](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![图片展示的是LeroBot数据集可视化界面。左侧为时间轴，可拖拽查看不同时刻画面。中间是摄像头画面，显示双手在 自动生成](../../en/images/d29-04.png)
</column>
</grid>

观察：指令和状态是不一致的，指令由主动臂Leader提供，状态由从动臂Follower提供

## 可视化查看指定episode

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![图片展示的是neron.io平台中可视化查看指定episode的界面。左侧为数据集结构，显示有observation_images等数据。中间上方是实时摄像头画面，画面中有一个橙子。右侧是数据曲线图，展示了不同数据随时间的变化情况。底部是时间轴，可拖拽查看任意时刻的数据。该图片与上文“可视化查看指定episode”内容对应，直观呈现了查看指定episode时的界面及数据展示情况。](../../en/images/d29-05.png)

拖拽时间轴查看任意时刻的摄像头画面和舵机位置

## 回放指定episode从动臂动作

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

能听到声音`Replaying episode`，然后从动臂移动，回放复现指定episode的动作

其实到这里，已经能唬住很多外行了，不是吗

<figure view-type="Preview">[附件 / Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>