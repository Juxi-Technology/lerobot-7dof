[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | 한국어 | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# 데이터셋 보기 및 재생

## 전체 데이터셋 시각화

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

`TommyZihao/lerobot_zihao_dataset_a` 또는 다른 데이터셋을 입력합니다

![이 이미지는 LeRobot Dataset Visualizer 인터페이스를 보여 주며, 화면 안에 로봇이 있고 상단에는 "LeRobot Dataset Visualizer"라는 문구가 있습니다. 가운데에는 "TommyZihao/lerobot_zihao_dataset_a" 같은 데이터셋 옵션을 보여 주는 드롭다운 메뉴가 있고, 그 아래에 "Example Datasets"와 데이터셋 이름들이 있으며, 하단에는 파란색 "Explore Open Datasets" 버튼이 있습니다. 이 이미지는 위에서 언급한 전체 데이터셋 시각화와 관련이 있으며, 지정된 데이터셋을 입력하는 동작에 해당합니다.](../../en/images/d29-01.png)

![addCriterion addCriterion를 보여 주는 이미지](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![이 이미지는 LeRobot 오렌지 집기 데이터셋의 시각화 인터페이스를 보여 줍니다. 상단에는 오렌지를 집는 비디오가 있으며, 오렌지가 흰색 물체에 대고 잡혀 있습니다. 아래에는 "actuator", "gripper", "gripper_pos" 같은 여러 변수의 시간에 따른 곡선을 보여 주는 데이터 차트가 있습니다. 왼쪽에는 지시 목록이 있고 현재 "Grab Orangesanges"가 선택되어 있습니다. 재생 및 일시 정지 버튼은 오른쪽 하단에 있습니다. 이 이미지는 특정 에피소드 시각화와 관련이 있으며, 집기 동작과 그에 해당하는 데이터를 시각적으로 보여 줍니다.](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![이 이미지는 LeRobot 데이터셋 시각화 인터페이스를 보여 줍니다. 왼쪽은 서로 다른 시점의 화면을 보기 위해 드래그할 수 있는 타임라인이고, 가운데는 두 손이 움직이는 모습을 보여 주는 카메라 화면입니다.](../../en/images/d29-04.png)
</column>
</grid>

관찰: 명령과 상태는 다릅니다 — 명령은 Leader 암이 제공하고, 상태는 Follower 암이 제공합니다

## 특정 에피소드 시각화

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![이 이미지는 rerun.io 플랫폼에서 특정 에피소드를 시각화하는 인터페이스를 보여 줍니다. 왼쪽은 observation_images 같은 데이터를 보여 주는 데이터셋 구조입니다. 가운데 상단은 라이브 카메라 화면으로 화면 안에 오렌지가 있습니다. 오른쪽은 서로 다른 데이터가 시간에 따라 어떻게 변하는지 보여 주는 데이터 곡선입니다. 하단은 언제든 임의 시점의 데이터를 보기 위해 드래그할 수 있는 타임라인입니다. 이 이미지는 "특정 에피소드 시각화"에 해당하며, 특정 에피소드를 볼 때의 인터페이스와 데이터를 시각적으로 보여 줍니다.](../../en/images/d29-05.png)

타임라인을 드래그하여 언제든 임의 시점의 카메라 화면과 서보 위치를 볼 수 있습니다

## 특정 에피소드의 Follower 암 동작 재생

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

`Replaying episode`라는 메시지가 들리고, 그다음 Follower 암이 움직이며 지정된 에피소드의 동작을 재생하여 재현합니다

사실 이쯤 되면 이미 많은 일반인에게 깊은 인상을 줄 수 있지 않습니까

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
