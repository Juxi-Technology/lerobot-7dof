[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | 繁體中文 | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Ubuntu電腦

## 連接攝影機和電腦

```Shell
lerobot-find-cameras opencv
```

![這張圖片展示了Ubuntu系統下終端機裡的攝影機偵測結果，終端機中執行了「lerobot-find-cameras opencv」的命令，偵測到編號為Camera #0的攝影機，該攝影機名稱為OpenCV Camera，路徑為/dev/video0，類型為OpenCV，後端api為V4L2，其預設串流格式的相關參數包括Fourcc格式為YUYV、寬640、高480、影格率30.0，最後還顯示影像儲存已完成，相關圖片已儲存至outputs/captured_images目錄，這對應了文件中Ubuntu電腦尋找連接的攝影機的相關操作內容。](../../en/images/d20-01.png)

## 遠端操作並顯示攝影機畫面

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

## 多個攝影機，遠端操作並顯示攝影機畫面

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
