[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | 한국어 | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# 시연으로 데이터셋 수집하기

## 같은 이름의 기존 데이터셋 삭제(있을 경우)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## 카메라 한 대, 데이터셋 수집 - Mac 컴퓨터

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

## 카메라 두 대, 데이터셋 수집 - Mac 컴퓨터

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

## 수집 중

<grid>
<column width-ratio="0.508765">
![이 이미지는 Mac에서 OpenVSLAM으로 데이터셋을 수집하는 동안의 터미널 인터페이스를 보여 줍니다. 상단에는 해상도, 프레임 레이트, 인코더 같은 수집 매개변수가 표시됩니다. 아래는 수집 로그로, 수집 시작 시각, 버전 정보, 스레드 수, 인코더를 기록하고 298/298 에피소드 수집, 총 5119.33초 같은 수집 진행 상황도 보여 줍니다. 하단에는 "ESC" 키 관련 안내가 있는데, 즉시 중지하고 데이터셋을 업로드하는 것에 대한 내용입니다. 이 이미지는 데이터셋 수집 워크플로와 관련이 있으며, 수집 중의 터미널 피드백을 시각적으로 보여 줍니다.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![이 이미지는 Mac의 명령 줄 터미널 인터페이스를 보여 주며, 카메라 데이터셋 수집과 관련된 실행 로그 정보를 표시하는 데 사용됩니다. config parameters, 인코딩 라이브러리 버전, 각 구성 항목(key frame, CRF, 인코딩 해상도 등)의 값 같은 SVT 관련 구성 매개변수를 포함하고, MP4 파일 처리에 대한 메시지, 프로그램 실행 중의 장치 연결 해제 기록과 타임스탬프 같은 런타임 상태 로그도 보여 줍니다. 전반적으로 카메라 데이터셋 수집 중의 백그라운드 실행 상태를 나타냅니다.](../../en/images/d26-02.png)
</column>
</grid>

키보드 화살표 키 조작:  
→ (오른쪽 화살표) 현재 에피소드를 조기에 종료하고 다음 에피소드로 넘어갑니다.  
← (왼쪽 화살표) 현재 에피소드를 취소하고 다시 기록합니다.  
ESC, 즉시 중지하고, 비디오를 인코딩한 뒤 데이터셋을 업로드합니다.

## 수집 완료 — 데이터셋 저장 디렉터리

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
