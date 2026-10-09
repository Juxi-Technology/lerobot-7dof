[English](../en/05-teleoperation/ubuntu.md) | 简体中文 | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Ubuntu电脑

## 给端口赋予权限

让所有用户都有权限读写这些串口设备

```Shell
sudo chmod 666 /dev/ttyACM*
```

## 遥操作

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![图片展示的是在Ubuntu电脑上给端口赋予权限及遥操作的相关操作界面。首先执行`sudo chmod 666 /dev/ttyACM*`命令，给串口设备赋予权限。接着输入`lerobot-teleoperate`命令，显示了robot和teleop的相关信息，如robot的id为“zihao follower arm”，port为“/dev/ttyACM0”，teleop的id为“zihao leader arm”，port为“/dev/ttyACM1”。该图片与上下文介绍的给端口赋予权限及遥操作的内容紧密相关，直观呈现了操作过程及结果。](../../en/images/d16-01.png)