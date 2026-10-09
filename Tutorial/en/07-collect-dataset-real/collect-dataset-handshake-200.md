English | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# Collecting a Dataset by Demonstration — Handshake 200

## Create a Dataset Repo on HuggingFace

https://huggingface.co/new-dataset

![The image shows the interface for creating a new dataset repository on HuggingFace. "Owner" is shown as TommyZihao, the "Dataset name" field is filled with "lerobot_zihao_dataset_shake200", "License" is set to mit, and the "Public" option is selected, with a note below that anyone can view this dataset while only the dataset owner or organization members can commit. At the bottom it notes that after creating the dataset you can upload files through the web interface or git, and there is a "Create dataset" button. This image relates to the content on creating a Dataset Repo on HuggingFace, and shows the interface for the create-dataset operation.](../../en/images/d27-01.png)

## Delete Any Existing Dataset with the Same Name (If Any)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Shake200 Dataset Collection

One camera, collecting a dataset - Mac computer

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

## During Collection

<grid>
<column width-ratio="0.508765">
![This is the terminal screen recording during collection of the handshake dataset (Shake200). Both the top and lower-middle of the interface show information labelled "INFO 202-04-15 17:00:22.227 Recording episode 36", indicating that the 36th data-collection episode is being recorded. It also presents the relevant parameters of the SVT-AV1 encoder and progress information "200/200 \[00:00:00::00, 4911.09 examples/s\]", reflecting that this stage is in the middle of dataset collection, corresponding to the "During Collection" step described in the document and showing the system's running state at collection time.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![This image is the Mac terminal's running interface, showing the configuration and runtime logs of the SVT project. The interface contains several configuration parameters, such as the SVT config's GOP size, mini-pod size, key-frame type and CRF values, as well as version information for the SVT-A1 Encoder library. The lower half's logs record video-file operations, mentioning moving the atom to the beginning of the file, plus the connect/disconnect states of several components such as OpenVCamera and SV280 follower, and finally shows the terminal-completion prompt for the current path, corresponding to the terminal running scenario during the demonstration collection of the handshake 200 dataset.](../../en/images/d27-02.png)
</column>
</grid>

Keyboard arrow key controls:  
→ (Right arrow) Terminate the current episode early; move on to the next episode.  
← (Left arrow) Cancel the current episode; record it again.  
ESC, stop immediately, encode the video, and upload the dataset.

## Collection Finished — Dataset Save Directory

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
