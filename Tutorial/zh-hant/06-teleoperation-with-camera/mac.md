[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | 繁體中文 | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Mac電腦

## 連接攝影機和電腦

```Shell
lerobot-find-cameras opencv
```

![圖片展示了Mac電腦連接攝影機後的偵測結果。畫面中列出兩個攝影機資訊，分別是外接的攝影機和Mac內建的前置攝影機。外接攝影機的Fps為60.00024，內建攝影機的Fps為30.0。該圖片與文件中介紹Mac電腦連接攝影機和電腦的內容相關，直觀呈現了連接後攝影機的偵測情況，協助使用者瞭解攝影機的類型、ID、後端API等資訊，以及各攝影機的影格率。](../../en/images/d21-01.png)

## 一個攝影機，遠端操作並顯示攝影機畫面

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

執行後啟動遠端操作

會開啟rerun.io畫面，即時顯示各個伺服馬達關節的軌跡，以及攝影機即時畫面

並將影像儲存至`~/用户名/outputs/captured_images`目錄

![這張rerun.io的介面畫面，對應文件中遠端操作攝影機功能的相關內容，是單攝影機遠端操作情境下的即時顯示介面。介面左側為折線圖區域，顯示了各個伺服馬達關節的軌跡變化曲線，帶有不同顏色標識的圖例區分各類關節資料；右側分為上下兩個視窗，分別展示了攝影機拍攝的室內即時畫面；底部則是時間軸與串流資訊區域，同時該情境支援將攝影機擷取的影像儲存至指定的`~/用户名/outputs/captured_images`目錄。](../../en/images/d21-02.png)

## 多個攝影機，遠端操作並顯示攝影機畫面

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
