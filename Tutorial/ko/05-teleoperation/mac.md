[English](../en/05-teleoperation/mac.md) | [简体中文](../zh-hans/05-teleoperation/mac.md) | [繁體中文](../zh-hant/05-teleoperation/mac.md) | [Deutsch](../de/05-teleoperation/mac.md) | [Español](../es/05-teleoperation/mac.md) | [Français](../fr/05-teleoperation/mac.md) | [Italiano](../it/05-teleoperation/mac.md) | [日本語](../ja/05-teleoperation/mac.md) | 한국어 | [Português (BR)](../pt-br/05-teleoperation/mac.md) | [Português (PT)](../pt-pt/05-teleoperation/mac.md)

# Mac 컴퓨터

## 포트에 권한 부여

모든 사용자에게 이 시리얼 장치의 읽기 및 쓰기 권한을 부여합니다

```Shell
chmod 666 /dev/tty.*
```

## 포트 번호 확인

Follower 암:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader 암:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## 텔레오퍼레이션

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm
```

## 다른 포트도 사용할 수 있습니다

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.wchusbserial5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.wchusbserial5AAF2194741 \
    --teleop.id=my_leader_arm
```
