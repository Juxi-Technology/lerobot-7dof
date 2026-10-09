[English](../en/04-calibrate-arm/mac.md) | 简体中文 | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Mac电脑

## 回顾端口号

从动臂：

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

主动臂：

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## 校准从动臂Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![图片展示的是在Mac电脑上使用命令行进行SO101舵机校准的界面。命令为“lerobot-calibrate”，参数包括robot.type、robot.port、robot.id等。界面显示了机器人相关配置信息，如“zihao_follower_arm”等。下方提示按“c”回车开始校准，还显示了“zihao_follower_arm SO101Follower connected”等信息。该图片与文档中“校准从动臂Follower”部分内容对应，直观呈现了校准操作的命令及界面反馈。](../../en/images/d13-01.png)

![图片展示的是在Ubuntu系统下使用命令行进行Lerobot校准操作的界面。命令为“lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm”，显示了从动臂的校准信息，包括各关节的最小、最大和当前位置等数据。界面中用红色框突出显示了“按回车开始校准”“依次转动每个关节达到每个关节的上下限位”“按回车结束校准”等关键操作提示，与上下文介绍的校准操作步骤相呼应。](../../en/images/d13-02.png)

## 校准主动臂Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![图片展示的是在Ubuntu系统下使用命令行进行Lerobot校准的界面。命令行中执行了“sudo chmod 666 /dev/ttyACM*”和“lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1”等操作，显示了从动臂和主动臂的端口号信息。界面中还提示按回车开始校准，依次转动每个关节达到上下限位，按回车结束校准，最后显示校准配置文件保存路径。该图片与文档中Lerobot校准内容相关，直观呈现了校准操作步骤。](../../en/images/d13-03.png)

## 查看校准配置文件

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## 常见Bug

- 找不到其中的一个或者几个舵机

![图片展示的是SOFollower校准过程中显示的舵机参数信息。上方显示了连接信息及校准提示，要求将从动臂移动到活动范围中间并按ENTER，然后按顺序通过所有关节的活动范围，录制位置，按ENTER停止。下方表格列出了shoulder_pan、shoulder_lift、elbow_flex、wrist_flex、gripper等舵机的NAME、MIN、POS、MAX值。该图片与文档中校准从动臂的内容相关，直观呈现了校准过程中的参数情况。](../../en/images/d13-04.png)



## 注意事项

### ①有一个臂达到限位后不动了

需要重新校准

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ②找不到舵机

![图片展示的是Mac电脑终端界面，显示了Lerobot机器人相关代码运行时的错误信息。错误提示FeetechMotorsBus电机检查在端口'/dev/tty.usbmodem5AAF2193061'失败，缺少电机ID - 1至 - 6，且预期模型为777。还列出全部预期电机列表和全部找到的电机列表。该图片与文档中“常见Bug”部分相关，直观呈现了找不到舵机这一问题在代码运行时的错误表现。](../../en/images/d13-05.png)

舵机电没插