English | [简体中文](../zh-hans/05-teleoperation/mac.md) | [繁體中文](../zh-hant/05-teleoperation/mac.md) | [Deutsch](../de/05-teleoperation/mac.md) | [Español](../es/05-teleoperation/mac.md) | [Français](../fr/05-teleoperation/mac.md) | [Italiano](../it/05-teleoperation/mac.md) | [日本語](../ja/05-teleoperation/mac.md) | [한국어](../ko/05-teleoperation/mac.md) | [Português (BR)](../pt-br/05-teleoperation/mac.md) | [Português (PT)](../pt-pt/05-teleoperation/mac.md)

# Mac Computer

## Grant Permissions to the Port

Give all users permission to read and write these serial devices

```Shell
chmod 666 /dev/tty.*
```

## Review the Port Numbers

Follower arm:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader arm:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Teleoperation

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm
```

## The Other Port Works Too

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.wchusbserial5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.wchusbserial5AAF2194741 \
    --teleop.id=my_leader_arm
```