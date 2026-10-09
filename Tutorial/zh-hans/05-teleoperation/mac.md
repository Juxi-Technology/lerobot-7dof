[English](../en/05-teleoperation/mac.md) | 简体中文 | [繁體中文](../zh-hant/05-teleoperation/mac.md) | [Deutsch](../de/05-teleoperation/mac.md) | [Español](../es/05-teleoperation/mac.md) | [Français](../fr/05-teleoperation/mac.md) | [Italiano](../it/05-teleoperation/mac.md) | [日本語](../ja/05-teleoperation/mac.md) | [한국어](../ko/05-teleoperation/mac.md) | [Português (BR)](../pt-br/05-teleoperation/mac.md) | [Português (PT)](../pt-pt/05-teleoperation/mac.md)

# Mac电脑

## 给端口赋予权限

让所有用户都有权限读写这些串口设备

```Shell
chmod 666 /dev/tty.*
```

## 回顾端口号

从动臂：

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

主动臂：

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## 遥操作

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm
```

## 用另一个端口也行

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.wchusbserial5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.wchusbserial5AAF2194741 \
    --teleop.id=my_leader_arm
```