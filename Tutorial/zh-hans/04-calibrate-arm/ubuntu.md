[English](../en/04-calibrate-arm/ubuntu.md) | 简体中文 | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Ubuntu电脑

## 给端口赋予权限

让所有用户都有权限读写这些串口设备

```Shell
sudo chmod 666 /dev/ttyACM*
```

## 校准从动臂Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![图片展示的是Ubuntu电脑上执行“lerobot-calibrate”命令校准从动臂Follower的终端界面。界面中显示了从动臂的连接信息、关节名称、上下限值等数据。关键信息有：按回车开始校准，依次转动每个关节达到上下限位，按回车结束校准；以及“Calibration saved to”等校准文件路径信息。该图片与文档中校准从动臂Follower的操作步骤紧密相关，直观呈现了校准过程中的终端反馈。](../../en/images/d12-01.png)

## 校准Leader主动臂

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![图片展示的是Ubuntu电脑给端口赋予权限后的界面。命令行中输入“sudo chmod 666 /dev/ttyACM*”，执行后显示“zihao_leader_arm”等信息。下方有“按回车开始校准”“依次转动每个关节达到每个关节的上下限位”“按回车结束校准”等提示，以及“Calibration saved to”等校准相关路径信息。该图片与上文“校准Leader主动臂”内容对应，直观呈现了校准前的准备工作界面。](../../en/images/d12-02.png)

## 查看校准配置文件

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![图片展示的是Ubuntu电脑终端界面中显示的“zihao_follower_arm.json”文件内容。文件包含多个臂的配置信息，如shoulder_pan、shoulder_lift、elbow_flex、wrist_flex等，每个臂有id、drive_mode、homing_offset、range_min、range_max等参数。该图片与上文“查看校准配置文件”内容相关，直观呈现了校准配置文件中的具体参数信息，帮助用户了解各臂的配置情况。](../../en/images/d12-03.png)



## 注意事项

### ①有一个臂达到限位后不动了

需要重新校准

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ②找不到舵机

![这是一张显示在Ubuntu系统终端中的报错界面截图，对应文档中“找不到舵机”的注意事项说明。界面提示发生了RuntimeError错误，具体为“FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'”，即舵机检查失败。界面还列出了预期的舵机信息，预期的电机ID为1-6，对应模型均为777，但最终找到的电机列表为空，结合上下文可知该报错是因舵机未通电导致的。](../../en/images/d12-04.png)

舵机电没插