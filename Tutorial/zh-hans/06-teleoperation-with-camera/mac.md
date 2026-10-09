[English](../en/06-teleoperation-with-camera/mac.md) | 简体中文 | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Mac电脑

## 连接摄像头和电脑

```Shell
lerobot-find-cameras opencv
```

![图片展示了Mac电脑连接摄像头后的检测结果。画面中列出两个摄像头信息，分别是外接的摄像头和Mac自带的前置摄像头。外接摄像头的Fps为60.00024，自带摄像头的Fps为30.0。该图片与文档中介绍Mac电脑连接摄像头和电脑的内容相关，直观呈现了连接后摄像头的检测情况，帮助用户了解摄像头的类型、ID、后端API等信息，以及各摄像头的帧率。](../../en/images/d21-01.png)

## 一个摄像头，遥操作并显示摄像头画面

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

运行后启动遥操作

会打开rerun.io画面，实时显示各个舵机关节的轨迹，以及摄像头实时画面

并保存图像至`~/用户名/outputs/captured_images`目录

![这张rerun.io的界面画面，对应文档中遥操作摄像头功能的相关内容，是单摄像头遥操作场景下的实时显示界面。界面左侧为折线图区域，显示了各个舵机关节的轨迹变化曲线，带有不同颜色标识的图例区分各类关节数据；右侧分为上下两个窗口，分别展示了摄像头拍摄的室内实时画面；底部则是时间轴与流信息区域，同时该场景支持将摄像头采集的图像保存至指定的`~/用户名/outputs/captured_images`目录。](../../en/images/d21-02.png)

## 多个摄像头，遥操作并显示摄像头画面

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
