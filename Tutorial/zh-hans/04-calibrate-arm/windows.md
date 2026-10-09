[English](../en/04-calibrate-arm/windows.md) | 简体中文 | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Windows电脑



<callout emoji="🚫">
需要同时接上主动臂和从动臂
</callout>

## 校准从动臂Follower

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![图片展示的是在Windows电脑上使用命令行进行lerobot校准操作的界面。命令为“lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm”。界面中显示了校准信息，包括“zihao_follower_arm SO10IFollower connected”等提示，还列出了机械臂各关节的NAME、MIN、POS、MAX值。校准过程中，提示用户将机械臂移动到其运动范围的中间位置并按ENTER，同时记录位置，按ENTER可停止。该图片与文档中校准从动臂的内容相关，展示了具体操作步骤及界面反馈。](../../en/images/d14-01.png)

## 校准主动臂Leader

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![图片展示的是在Windows电脑上使用lerobot-calibrate命令进行机械臂校准的命令行界面。界面中显示了校准从动臂和主动臂的相关信息，包括校准位置保存路径、机器人类型、端口号、ID等。还提示将从动臂移到运动范围中间并按ENTER，逐个关节通过其整个运动范围，记录位置，按ENTER停止。界面底部显示了各关节的名称、最小值、当前位置、最大值等数据。](../../en/images/d14-02.png)

## 文件导出位置

C:\Users\40743\\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## 换机械臂校准

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## 注意事项

### ①有一个臂达到限位后不动了

需要重新校准

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ②找不到舵机

![图片展示的是在macOS系统下运行lerobot程序时出现的错误信息。程序运行时，出现“FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'”的RuntimeError，提示舵机ID缺失，包括1 - 6号舵机，预期模型号均为777，但实际找到的舵机列表为空。这与文档中“找不到舵机”的注意事项相关，可能是由于舵机电没插，需重新插拔并转一转插口。](../../en/images/d14-03.png)

舵机电没插，重新插拔并转一转插口