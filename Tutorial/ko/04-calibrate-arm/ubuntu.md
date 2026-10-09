[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | 한국어 | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Ubuntu 컴퓨터

## 포트에 권한 부여

모든 사용자에게 이 시리얼 장치의 읽기 및 쓰기 권한을 부여합니다

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Follower 암 캘리브레이션

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![이 이미지는 Ubuntu 컴퓨터에서 "lerobot-calibrate" 명령을 실행하여 Follower 암을 캘리브레이션하는 터미널 인터페이스를 보여 줍니다. Follower의 연결 정보, 관절 이름, 상한/하한 값이 표시됩니다. 주요 정보로는 캘리브레이션을 시작하려면 Enter를 누르라는 안내, 각 관절을 차례로 상한과 하한까지 움직이라는 안내, 캘리브레이션을 끝내려면 Enter를 누르라는 안내, 그리고 "Calibration saved to" 및 그 밖의 캘리브레이션 파일 경로 정보가 있습니다. 이 이미지는 Follower 암을 캘리브레이션하는 단계와 밀접하게 관련되어 있으며, 캘리브레이션 중의 터미널 피드백을 시각적으로 보여 줍니다.](../../en/images/d12-01.png)

## Leader 암 캘리브레이션

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![이 이미지는 Ubuntu 컴퓨터에서 포트에 권한을 부여한 뒤의 인터페이스를 보여 줍니다. 명령 줄에 "sudo chmod 666 /dev/ttyACM*"을 입력했고, 실행 후 "zihao_leader_arm" 같은 정보가 표시되었습니다. 아래에는 "캘리브레이션을 시작하려면 Enter를 누르십시오", "각 관절을 차례로 상한과 하한까지 움직이십시오", "캘리브레이션을 끝내려면 Enter를 누르십시오" 같은 안내와 "Calibration saved to" 및 그 밖의 캘리브레이션 관련 경로 정보가 있습니다. 이 이미지는 "Leader 암 캘리브레이션" 절에 해당하며, 캘리브레이션 전의 준비 인터페이스를 시각적으로 보여 줍니다.](../../en/images/d12-02.png)

## 캘리브레이션 구성 파일 보기

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![이 이미지는 Ubuntu 터미널에 표시된 "zihao_follower_arm.json" 파일의 내용을 보여 줍니다. 파일에는 shoulder_pan, shoulder_lift, elbow_flex, wrist_flex 등 여러 암의 구성 정보가 포함되어 있으며, 각 암에는 id, drive_mode, homing_offset, range_min, range_max 같은 매개변수가 있습니다. 이 이미지는 "캘리브레이션 구성 파일 보기" 절과 관련이 있으며, 캘리브레이션 파일의 구체적인 매개변수 정보를 시각적으로 보여 주어 사용자가 각 암의 구성을 이해하는 데 도움을 줍니다.](../../en/images/d12-03.png)



## 참고 사항

### ① 한쪽 암이 한계에 도달한 뒤 움직이지 않음

다시 캘리브레이션해야 합니다

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② 서보를 찾을 수 없음

![이것은 Ubuntu 터미널의 오류 인터페이스를 보여 주는 스크린샷으로, "서보를 찾을 수 없음" 참고 사항에 해당합니다. 인터페이스에는 RuntimeError, 구체적으로 "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'", 즉 서보 검사가 실패했다고 보고합니다. 또한 예상 서보 정보도 나열되는데, 예상 모터 ID는 1-6이고 예상 모델은 777이지만 실제로 찾은 모터 목록은 비어 있습니다. 맥락과 종합해 볼 때 이 오류는 서보에 전원이 공급되지 않아 발생한 것입니다.](../../en/images/d12-04.png)

서보 전원이 꽂혀 있지 않습니다
