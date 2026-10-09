[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | 한국어 | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Mac 컴퓨터

## 카메라를 컴퓨터에 연결

```Shell
lerobot-find-cameras opencv
```

![이 이미지는 Mac 컴퓨터에 카메라를 연결한 뒤의 감지 결과를 보여 줍니다. 화면에는 두 대의 카메라 정보가 나열되어 있으며, 각각 외장 카메라와 Mac에 내장된 전면 카메라입니다. 외장 카메라의 Fps는 60.00024이고 내장 카메라의 Fps는 30.0입니다. 이 이미지는 문서에서 Mac 컴퓨터에 카메라를 연결하는 내용과 관련이 있으며, 연결 후 카메라의 감지 상황을 시각적으로 보여 주어 사용자가 카메라의 유형, ID, 백엔드 API 등의 정보와 각 카메라의 프레임 레이트를 이해하는 데 도움을 줍니다.](../../en/images/d21-01.png)

## 카메라 한 대, 카메라 화면을 표시하며 텔레오퍼레이션

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

실행하면 텔레오퍼레이션이 시작됩니다

rerun.io 창이 열리고, 각 서보 관절의 궤적이 실시간으로 표시되며 카메라 화면도 함께 보입니다

그리고 이미지를 `~/username/outputs/captured_images` 디렉터리에 저장합니다

![이 rerun.io 인터페이스 화면은 문서의 텔레오퍼레이션 카메라 기능 관련 내용에 대응하며, 카메라 한 대를 사용하는 텔레오퍼레이션 시나리오의 실시간 표시 화면입니다. 인터페이스 왼쪽은 꺾은선 그래프 영역으로, 각 서보 관절의 궤적 변화 곡선을 보여 주며 서로 다른 색상으로 표시된 범례로 각 관절 데이터를 구분합니다. 오른쪽은 위아래 두 개의 창으로 나뉘어 각각 카메라가 촬영한 실내 실시간 화면을 보여 줍니다. 아래쪽은 타임라인과 스트림 정보 영역이며, 이 시나리오에서는 카메라가 수집한 이미지를 지정된 `~/username/outputs/captured_images` 디렉터리에 저장하는 것을 지원합니다.](../../en/images/d21-02.png)

## 카메라 여러 대, 카메라 화면을 표시하며 텔레오퍼레이션

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
