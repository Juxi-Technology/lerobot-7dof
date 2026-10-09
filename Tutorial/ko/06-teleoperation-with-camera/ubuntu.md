[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | 한국어 | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Ubuntu 컴퓨터

## 카메라를 컴퓨터에 연결

```Shell
lerobot-find-cameras opencv
```

![이 이미지는 Ubuntu 터미널의 카메라 감지 결과를 보여 줍니다. "lerobot-find-cameras opencv" 명령을 실행하여 Camera #0 번호, OpenCV Camera 이름, /dev/video0 경로, OpenCV 유형, V4L2 백엔드 API를 가진 카메라를 감지했습니다. 기본 스트림 형식 매개변수로는 Fourcc 형식 YUYV, 너비 640, 높이 480, 프레임 레이트 30.0이 있습니다. 마지막으로 이미지 저장이 완료되었고 이미지가 outputs/captured_images 디렉터리에 저장되었음을 보여 줍니다. 이는 Ubuntu 컴퓨터에서 연결된 카메라를 찾는 내용에 해당합니다.](../../en/images/d20-01.png)

## 카메라 화면을 표시하며 텔레오퍼레이션

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

## 카메라 여러 대, 카메라 화면을 표시하며 텔레오퍼레이션

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
