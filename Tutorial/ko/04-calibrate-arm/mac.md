[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | 한국어 | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Mac 컴퓨터

## 포트 번호 확인

Follower 암:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader 암:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Follower 암 캘리브레이션

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![이 이미지는 Mac에서 SO101 서보를 캘리브레이션하는 명령 줄 인터페이스를 보여 줍니다. 명령은 "lerobot-calibrate"이며, 매개변수로 robot.type, robot.port, robot.id가 포함됩니다. 인터페이스에는 "zihao_follower_arm" 같은 로봇 구성 정보가 표시됩니다. 아래에서는 캘리브레이션을 시작하려면 "c"를 누르고 Enter를 누르라고 안내하며, "zihao_follower_arm SO101Follower connected" 같은 메시지도 보여 줍니다. 이 이미지는 "Follower 암 캘리브레이션" 절에 해당하며, 캘리브레이션 명령과 인터페이스 피드백을 시각적으로 보여 줍니다.](../../en/images/d13-01.png)

![이 이미지는 Ubuntu에서 LeRobot 캘리브레이션 작업의 명령 줄 인터페이스를 보여 줍니다. 명령은 "lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm"이며, 각 관절의 최솟값, 최댓값, 현재 위치를 포함한 Follower의 캘리브레이션 정보를 표시합니다. 주요 조작 안내가 빨간 박스로 강조되어 있는데, "캘리브레이션을 시작하려면 Enter를 누르십시오", "각 관절을 차례로 상한과 하한까지 움직이십시오", "캘리브레이션을 끝내려면 Enter를 누르십시오" 등이며, 본문에 설명된 캘리브레이션 단계와 호응합니다.](../../en/images/d13-02.png)

## Leader 암 캘리브레이션

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![이 이미지는 Ubuntu에서 LeRobot 캘리브레이션의 명령 줄 인터페이스를 보여 줍니다. 명령 줄에서 "sudo chmod 666 /dev/ttyACM*"과 "lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1" 같은 작업을 실행하여 Follower 및 Leader 암의 포트 번호 정보를 표시했습니다. 인터페이스는 또한 캘리브레이션을 시작하려면 Enter를 누르고, 각 관절을 차례로 상한과 하한까지 움직이고, 끝내려면 Enter를 누르라고 안내하며, 마지막으로 캘리브레이션 구성 파일이 저장된 경로를 보여 줍니다. 이 이미지는 LeRobot 캘리브레이션 내용과 관련이 있으며, 캘리브레이션 단계를 시각적으로 보여 줍니다.](../../en/images/d13-03.png)

## 캘리브레이션 구성 파일 보기

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## 자주 발생하는 문제

- 하나 또는 여러 개의 서보를 찾을 수 없음

![이 이미지는 SO Follower를 캘리브레이션하는 동안 표시되는 서보 매개변수 정보를 보여 줍니다. 상단에는 연결 정보와 캘리브레이션 안내가 있으며, Follower를 가동 범위의 중간으로 옮기고 ENTER를 누르라고 요청한 뒤, 모든 관절의 가동 범위를 순서대로 통과하여 위치를 기록하고 ENTER를 눌러 중지하라고 안내합니다. 아래 표에는 shoulder_pan, shoulder_lift, elbow_flex, wrist_flex, gripper 같은 서보의 NAME, MIN, POS, MAX 값이 나열되어 있습니다. 이 이미지는 Follower 암 캘리브레이션과 관련이 있으며, 캘리브레이션 중의 매개변수를 시각적으로 보여 줍니다.](../../en/images/d13-04.png)



## 참고 사항

### ① 한쪽 암이 한계에 도달한 뒤 움직이지 않음

다시 캘리브레이션해야 합니다

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② 서보를 찾을 수 없음

![이 이미지는 LeRobot 로봇 코드를 실행할 때의 오류 메시지가 있는 Mac 터미널 인터페이스를 보여 줍니다. 오류는 '/dev/tty.usbmodem5AAF2193061' 포트에서 FeetechMotorsBus 모터 검사가 실패했으며, 모터 ID -1부터 -6까지 누락되고 예상 모델은 777이라고 표시합니다. 또한 예상 모터의 전체 목록과 실제로 찾은 모터의 전체 목록을 나열합니다. 이 이미지는 "자주 발생하는 문제" 절과 관련이 있으며, "서보를 찾을 수 없음" 문제가 런타임 오류로 어떻게 나타나는지 시각적으로 보여 줍니다.](../../en/images/d13-05.png)

서보 전원이 꽂혀 있지 않습니다
