[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | 繁體中文 | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Ubuntu電腦

## 為連接埠賦予權限

讓所有使用者都有權限讀寫這些序列埠裝置

```Shell
sudo chmod 666 /dev/ttyACM*
```

## 遠端操作

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![圖片展示的是在Ubuntu電腦上為連接埠賦予權限及遠端操作的相關操作介面。首先執行`sudo chmod 666 /dev/ttyACM*`命令，為序列埠裝置賦予權限。接著輸入`lerobot-teleoperate`命令，顯示了robot和teleop的相關資訊，如robot的id為「zihao follower arm」，port為「/dev/ttyACM0」，teleop的id為「zihao leader arm」，port為「/dev/ttyACM1」。該圖片與上下文介紹的為連接埠賦予權限及遠端操作的內容緊密相關，直觀呈現了操作過程及結果。](../../en/images/d16-01.png)
