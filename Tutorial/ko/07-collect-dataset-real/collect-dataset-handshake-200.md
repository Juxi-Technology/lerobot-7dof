[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | 한국어 | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# 시연으로 데이터셋 수집하기 — Handshake 200

## HuggingFace에 데이터셋 저장소 생성

https://huggingface.co/new-dataset

![이 이미지는 HuggingFace에서 새 데이터셋 저장소를 만드는 인터페이스를 보여 줍니다. "Owner"는 TommyZihao로, 데이터셋 이름은 "lerobot_zihao_dataset_shake200", "License"는 mit로 설정되어 있고, "Public" 옵션이 선택되어 있어 누구나 볼 수 있으며 데이터셋 소유자나 조직 구성원만 커밋할 수 있습니다. 아래에는 데이터셋을 만든 뒤 웹 인터페이스나 git을 통해 파일을 업로드할 수 있다는 안내와 함께 하단에 "Create dataset" 버튼이 있습니다. 이 이미지는 HuggingFace에 데이터셋 저장소를 만드는 내용과 관련이 있으며, 데이터셋 생성 작업의 인터페이스를 보여 줍니다.](../../en/images/d27-01.png)

## 같은 이름의 기존 데이터셋 삭제(있을 경우)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Shake200 데이터셋 수집

카메라 한 대, 데이터셋 수집 - Mac 컴퓨터

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

## 수집 중

<grid>
<column width-ratio="0.508765">
![이것은 핸드셰이크 데이터셋(Shake200)을 수집할 때 화면을 녹화한 터미널 인터페이스 내용입니다. 인터페이스 상단과 중간에서 조금 아래쪽에 모두 "INFO 202-04-15 17:00:22.227 Recording episode 36" 정보가 표시되어 36번째 데이터 수집 주기(episode)를 녹화 중임을 보여 주며, 동시에 SVT-AV1 인코더 관련 파라미터와 진행 정보 "200/200 \[00:00:00::00, 4911.09 examples/s\]"도 나타납니다. 이는 이 단계가 데이터셋 수집 과정 중임을 나타내며, 문서에서 설명한 "수집 중" 단계에 대응하여 수집 시의 시스템 동작 상태를 보여 줍니다.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![이 이미지는 Mac 터미널의 실행 인터페이스로, SVT 프로젝트의 구성과 실행 로그 정보를 보여 줍니다. 인터페이스에는 SVT 구성의 GOP 크기, mini-pod 크기, 키프레임 유형, CRF 등의 값과 SVT-A1 Encoder 라이브러리의 버전 정보 등 여러 구성 파라미터가 포함되어 있습니다. 인터페이스 아래쪽 로그에는 비디오 파일 작업 관련 내용이 기록되어 있으며, 원자를 파일 시작 부분으로 이동한다는 내용과 여러 OpenVCamera, SV280 follower 등 구성 요소의 연결 및 해제 상태가 있고, 마지막에는 현재 경로에서의 터미널 실행 종료 안내가 표시됩니다. 이는 시연으로 핸드셰이크 200 데이터셋을 수집하는 과정의 터미널 실행 장면에 대응합니다.](../../en/images/d27-02.png)
</column>
</grid>

키보드 화살표 키 조작:  
→ (오른쪽 화살표) 현재 에피소드를 조기에 종료하고 다음 에피소드로 넘어갑니다.  
← (왼쪽 화살표) 현재 에피소드를 취소하고 다시 기록합니다.  
ESC, 즉시 중지하고, 비디오를 인코딩한 뒤 데이터셋을 업로드합니다.

## 수집 완료, 데이터셋 저장 디렉터리

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
