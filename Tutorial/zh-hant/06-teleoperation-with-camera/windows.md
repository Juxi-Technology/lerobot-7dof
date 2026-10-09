[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | 繁體中文 | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Windows電腦

## 連接攝影機和電腦

```Shell
lerobot-find-cameras opencv
```

![這張圖片是Windows系統的命令列報錯介面，對應文件中提到的「攝影機連接不上」的報錯情境。介面顯示的報錯資訊包含兩部分，一是全域報錯「Camera index out of range」，二是lerobot相關指令碼報錯「Failed to connect or configure OpenCV camera 0」。介面還列出了偵測到的攝影機資訊，共有2個名為OpenCV Camera的裝置，兩者均採用DSHOW後端，格式為YUY2，寬640、高480，影格率0.0FPS。該報錯內容與文件中描述的攝影機無法連接、在騰訊會議中卻能正常使用的情況對應。](../../en/images/d22-01.png)

## 遠端操作並顯示攝影機畫面

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

會開啟rerun.io畫面，即時顯示各個伺服馬達關節的軌跡，以及攝影機即時畫面

並將影像儲存至`C:\Users\用户\outputs\captured_images`目錄

![圖片展示的是rerun.io畫面，即時顯示了各個伺服馬達關節的軌跡及攝影機即時畫面。畫面左側為藍圖介面，有「teleoperation」等選項。中間是軌跡圖，顯示了「observation_wrist_rot.pos」等關節位置資料。右側是攝影機畫面，呈現了機器人視角下的場景。畫面右上角顯示「Waiting for data on rerun: http://127.0.0.1:9876/remote...」，下方有資料來源資訊。該圖與文件中介紹rerun.io畫面即時顯示攝影機畫面的內容相關，直觀呈現了畫面效果。](../../en/images/d22-02.png)

## 如果遇到了下面這種報錯

攝影機連接不上，但在騰訊會議中切換攝影機，仍然能正常開啟

![圖片展示的是Windows電腦命令列介面，顯示了攝影機偵測結果。介面上方有「管理員：命令提示字元 - C:\\Users\\40743\\miniconda3\\condabin\\conda.bat activate leroobot」字樣。下方輸出了「Detected Cameras」資訊，列出「Camera #0」為OpenCV Camera @ 0，Type為OpenCV，Backend api為DSHOW，Default stream profile等參數，還顯示了「ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)」等錯誤提示。該圖片與文件中遇到攝影機連接不上問題的上下文相關，直觀呈現了相關錯誤資訊。](../../en/images/d22-03.png)

修改`lerobot\src\lerobot\cameras\utils.py`檔案，將OpenCV後端改為`cv2.CAP_SHOW`

![這張圖片展示了一段Python程式碼，是用於修改lerobot程式庫中`utils.py`檔案的部分內容，該內容對應文件中針對Windows電腦連接攝影機報錯的解決步驟。程式碼定義了`get_cv2_backend()`函式，針對不同系統設定OpenCV後端：當系統為Windows時，原本註解的`return int(cv2.CAP_MSMF)`被註解掉，目前醒目提示的內容是`return int(cv2.CAP_DSHOW)`，旁邊標註說明使用該方案替代AVFOUNDATION；該函式還對macOS和Linux系統設定了對應的OpenCV後端方案，是解決指定攝影機連接問題的核心程式碼修改內容。](../../en/images/d22-04.png)

> 這是一個豆包都無法解決的bug，都怪lerobot程式庫封裝的太深了，初學者小白很難dubug

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## 連接多個攝影機，遠端操作並顯示攝影機畫面

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
