[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | 한국어 | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Windows 컴퓨터



<callout emoji="🚫">
Leader 암과 Follower 암을 반드시 모두 연결해야 합니다
</callout>

## Follower 암 캘리브레이션

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![이 이미지는 Windows 컴퓨터에서 lerobot 캘리브레이션을 실행하는 명령 줄 인터페이스를 보여 줍니다. 명령은 "lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm"입니다. 인터페이스에는 "zihao_follower_arm SO10IFollower connected" 같은 안내를 포함한 캘리브레이션 정보가 표시되고, 로봇 암의 각 관절의 NAME, MIN, POS, MAX 값도 나열됩니다. 캘리브레이션 중에는 로봇 암을 가동 범위의 중간으로 옮기고 ENTER를 누르라고 안내하면서 위치를 기록하고, ENTER를 눌러 중지하라고 합니다. 이 이미지는 Follower 암 캘리브레이션과 관련이 있으며, 구체적인 단계와 인터페이스 피드백을 보여 줍니다.](../../en/images/d14-01.png)

## Leader 암 캘리브레이션

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![이 이미지는 Windows 컴퓨터에서 lerobot-calibrate 명령으로 로봇 암을 캘리브레이션하는 명령 줄 인터페이스를 보여 줍니다. 캘리브레이션 위치 저장 경로, 로봇 유형, 포트 번호와 ID를 포함하여 Follower 및 Leader 암을 캘리브레이션하는 정보가 표시됩니다. 또한 Follower를 가동 범위의 중간으로 옮기고 ENTER를 누르고, 각 관절을 전체 가동 범위까지 통과시키고, 위치를 기록하고, ENTER를 눌러 중지하라고 안내합니다. 하단에는 각 관절의 이름, 최솟값, 현재 위치, 최댓값이 표시됩니다.](../../en/images/d14-02.png)

## 파일이 내보내지는 위치

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## 다른 로봇 암 캘리브레이션

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## 참고 사항

### ① 한쪽 암이 한계에 도달한 뒤 움직이지 않음

다시 캘리브레이션해야 합니다

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② 서보를 찾을 수 없음

![이 이미지는 macOS에서 lerobot 프로그램을 실행할 때의 오류 메시지를 보여 줍니다. 프로그램이 실행되는 동안 RuntimeError가 나타납니다: "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'", 이는 1-6번 서보를 포함한 누락된 서보 ID를 나타내며, 모두 예상 모델 번호가 777이지만 실제로 찾은 서보 목록은 비어 있습니다. 이는 "서보를 찾을 수 없음" 참고 사항과 관련이 있으며, 서보가 꽂혀 있지 않아 발생했을 수 있으므로 다시 꽂고 커넥터를 돌려 보십시오.](../../en/images/d14-03.png)

서보 전원이 꽂혀 있지 않습니다. 다시 꽂고 커넥터를 돌려 보십시오
