English | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# Collecting a Dataset by Demonstration

## Delete Any Existing Dataset with the Same Name (If Any)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## One Camera, Collecting a Dataset - Mac computer

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

## Two Cameras, Collecting a Dataset - Mac computer

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

## During Collection

<grid>
<column width-ratio="0.508765">
![The image shows the terminal interface while collecting a dataset with OpenVSLAM on a Mac. At the top it shows the collection parameters, such as resolution, frame rate and encoder. Below is the collection log, recording the collection start time, version information, thread count and encoder, and also showing collection progress, such as 298/298 episodes collected, 5119.33 seconds in total. At the bottom there are notes for the "ESC" key, such as stopping immediately and uploading the dataset. This image relates to the dataset collection workflow, visually presenting the terminal feedback during collection.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![This image shows the command line terminal interface on a Mac, used to display run log information related to camera dataset collection. It contains SVT-related configuration parameters, such as config parameters, the encoding library version and the values of each configuration item (such as key frame and CRF, encoding resolution), and also shows runtime status logs, such as messages about MP4 file processing, records of device disconnection and timestamps while the program runs. Overall it presents the background running state during camera dataset collection.](../../en/images/d26-02.png)
</column>
</grid>

Keyboard arrow key controls:  
→ (Right arrow) Terminate the current episode early; move on to the next episode.  
← (Left arrow) Cancel the current episode; record it again.  
ESC, stop immediately, encode the video, and upload the dataset.

## Collection Finished — Dataset Save Directory

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```





## Handshake

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