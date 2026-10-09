English | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# Viewing and Replaying a Dataset

## Visualize the Entire Dataset

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

Enter `TommyZihao/lerobot_zihao_dataset_a`, or another dataset

![The image shows the LeRobot Dataset Visualizer interface, with a robot in the picture and the words "LeRobot Dataset Visualizer" at the top. In the middle there is a dropdown menu showing dataset options such as "TommyZihao/lerobot_zihao_dataset_a", along with "Example Datasets" and dataset names under it, and a blue "Explore Open Datasets" button below. This image relates to visualizing the entire dataset mentioned above, corresponding to the operation of entering the specified dataset.](../../en/images/d29-01.png)

![Image showing addCriterion addCriterion](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![The image shows the visualization interface of the LeRobot grab-orange dataset. At the top is a video of grabbing an orange, with the orange held against a white object. Below are data charts showing curves of several variables over time, such as "actuator", "gripper" and "gripper_pos". On the left is a list of instructions, with "Grab Orangesanges" currently selected. Play and pause buttons are in the bottom right. This image relates to visualizing a specific episode, visually presenting the grabbing action and its corresponding data.](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![The image shows the LeRobot dataset visualization interface. On the left is a timeline that can be dragged to view the picture at different moments, and in the middle is the camera feed showing two hands in motion.](../../en/images/d29-04.png)
</column>
</grid>

Observation: the command and the state are not the same — the command is provided by the Leader arm, and the state is provided by the Follower arm

## Visualize a Specific Episode

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![The image shows the interface for visualizing a specific episode on the rerun.io platform. On the left is the dataset structure, showing data such as observation_images. In the middle top is the live camera feed, with an orange in the picture. On the right are data curves showing how different data changes over time. At the bottom is a timeline that can be dragged to view the data at any moment. This image corresponds to "Visualize a Specific Episode", visually presenting the interface and data when viewing a specific episode.](../../en/images/d29-05.png)

Drag the timeline to view the camera feed and servo positions at any moment

## Replay the Follower Arm's Motion for a Specific Episode

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

You will hear `Replaying episode`, then the Follower arm moves, replaying and reproducing the motion of the specified episode

Actually, by this point you can already impress a lot of laypeople, can't you

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>