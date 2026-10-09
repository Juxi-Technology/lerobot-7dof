English | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Ubuntu Computer

## Grant Permissions to the Port

Give all users permission to read and write these serial devices

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Teleoperation

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![The image shows the interface on an Ubuntu computer for granting permissions to the port and for teleoperation. First the command `sudo chmod 666 /dev/ttyACM*` is run to grant permissions to the serial devices. Then the `lerobot-teleoperate` command is entered, showing the robot and teleop information, such as the robot's id "zihao follower arm" and port "/dev/ttyACM0", and the teleop's id "zihao leader arm" and port "/dev/ttyACM1". This image is closely related to the content on granting port permissions and teleoperation, visually presenting the operation and its result.](../../en/images/d16-01.png)