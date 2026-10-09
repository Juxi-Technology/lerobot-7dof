[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | 한국어 | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Ubuntu 컴퓨터

## 포트에 권한 부여

모든 사용자에게 이 시리얼 장치의 읽기 및 쓰기 권한을 부여합니다

```Shell
sudo chmod 666 /dev/ttyACM*
```

## 텔레오퍼레이션

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![이 이미지는 Ubuntu 컴퓨터에서 포트에 권한을 부여하고 텔레오퍼레이션을 수행하는 인터페이스를 보여 줍니다. 먼저 `sudo chmod 666 /dev/ttyACM*` 명령을 실행하여 시리얼 장치에 권한을 부여합니다. 그다음 `lerobot-teleoperate` 명령을 입력하면 로봇의 id "zihao follower arm"과 포트 "/dev/ttyACM0", teleop의 id "zihao leader arm"과 포트 "/dev/ttyACM1" 같은 로봇 및 teleop 정보가 표시됩니다. 이 이미지는 포트 권한 부여와 텔레오퍼레이션 내용과 밀접하게 관련되어 있으며, 동작과 그 결과를 시각적으로 보여 줍니다.](../../en/images/d16-01.png)
