[English](../en/06-teleoperation-with-camera/windows.md) | 简体中文 | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Windows电脑

## 连接摄像头和电脑

```Shell
lerobot-find-cameras opencv
```

![这张图片是Windows系统的命令行报错界面，对应文档中提到的“摄像头连接不上”的报错场景。界面显示的报错信息包含两部分，一是全局报错“Camera index out of range”，二是lerobot相关脚本报错“Failed to connect or configure OpenCV camera 0”。界面还列出了检测到的摄像头信息，共有2个名为OpenCV Camera的设备，两者均采用DSHOW后端，格式为YUY2，宽640、高480，帧率0.0FPS。该报错内容与文档中描述的摄像头无法连接、在腾讯会议却能正常使用的情况对应。](../../en/images/d22-01.png)

## 遥操作并显示摄像头画面

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

会打开rerun.io画面，实时显示各个舵机关节的轨迹，以及摄像头实时画面

并保存图像至`C:\Users\用户\outputs\captured_images`目录

![图片展示的是rerun.io画面，实时显示了各个舵机关节的轨迹及摄像头实时画面。画面左侧为蓝图界面，有“teleoperation”等选项。中间是轨迹图，显示了“observation_wrist_rot.pos”等关节位置数据。右侧是摄像头画面，呈现了机器人视角下的场景。画面右上角显示“Waiting for data on rerun: http://127.0.0.1:9876/remote...”，下方有数据源信息。该图与文档中介绍rerun.io画面实时显示摄像头画面的内容相关，直观呈现了画面效果。](../../en/images/d22-02.png)

## 如果遇到了下面这种报错

摄像头连接不上，但在腾讯会议中切换摄像头，仍然能正常开启

![图片展示的是Windows电脑命令行界面，显示了摄像头检测结果。界面上方有“管理员：命令提示符 - C:\\Users\\40743\\miniconda3\\condabin\\conda.bat activate leroobot”字样。下方输出了“Detected Cameras”信息，列出“Camera #0”为OpenCV Camera @ 0，Type为OpenCV，Backend api为DSHOW，Default stream profile等参数，还显示了“ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)”等错误提示。该图片与文档中遇到摄像头连接不上问题的上下文相关，直观呈现了相关错误信息。](../../en/images/d22-03.png)

修改`lerobot\src\lerobot\cameras\utils.py`文件，将OpenCV后端改为`cv2.CAP_SHOW`

![这张图片展示了一段Python代码，是用于修改lerobot库中`utils.py`文件的部分内容，该内容对应文档中针对Windows电脑连接摄像头报错的解决步骤。代码定义了`get_cv2_backend()`函数，针对不同系统设置OpenCV后端：当系统为Windows时，原本注释的`return int(cv2.CAP_MSMF)`被注释掉，当前高亮标注的内容是`return int(cv2.CAP_DSHOW)`，旁边标注说明使用该方案替代AVFOUNDATION；该函数还对macOS和Linux系统设置了对应的OpenCV后端方案，是解决指定摄像头连接问题的核心代码修改内容。](../../en/images/d22-04.png)

> 这是一个豆包都无法解决的bug，都怪lerobot库封装的太深了，初学者小白很难dubug

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## 连接多个摄像头，遥操作并显示摄像头画面

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
