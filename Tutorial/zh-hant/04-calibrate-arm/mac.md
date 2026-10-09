[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | 繁體中文 | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Mac電腦

## 回顧連接埠號

從動臂：

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

主動臂：

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## 校正從動臂Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![圖片展示的是在Mac電腦上使用命令列進行SO101伺服馬達校正的介面。命令為「lerobot-calibrate」，參數包括robot.type、robot.port、robot.id等。介面顯示了機器人相關設定資訊，如「zihao_follower_arm」等。下方提示按「c」Enter開始校正，還顯示了「zihao_follower_arm SO101Follower connected」等資訊。該圖片與文件中「校正從動臂Follower」部分內容對應，直觀呈現了校正操作的命令及介面回饋。](../../en/images/d13-01.png)

![圖片展示的是在Ubuntu系統下使用命令列進行Lerobot校正操作的介面。命令為「lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm」，顯示了從動臂的校正資訊，包括各關節的最小、最大和目前位置等資料。介面中用紅色框醒目顯示了「按Enter開始校正」「依序轉動每個關節達到每個關節的上下限位」「按Enter結束校正」等關鍵操作提示，與上下文介紹的校正操作步驟相呼應。](../../en/images/d13-02.png)

## 校正主動臂Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![圖片展示的是在Ubuntu系統下使用命令列進行Lerobot校正的介面。命令列中執行了「sudo chmod 666 /dev/ttyACM*」和「lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1」等操作，顯示了從動臂和主動臂的連接埠號資訊。介面中還提示按Enter開始校正，依序轉動每個關節達到上下限位，按Enter結束校正，最後顯示校正設定檔儲存路徑。該圖片與文件中Lerobot校正內容相關，直觀呈現了校正操作步驟。](../../en/images/d13-03.png)

## 檢視校正設定檔

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## 常見Bug

- 找不到其中的一個或者幾個伺服馬達

![圖片展示的是SOFollower校正過程中顯示的伺服馬達參數資訊。上方顯示了連接資訊及校正提示，要求將從動臂移動到活動範圍中間並按ENTER，然後依順序通過所有關節的活動範圍，錄製位置，按ENTER停止。下方表格列出了shoulder_pan、shoulder_lift、elbow_flex、wrist_flex、gripper等伺服馬達的NAME、MIN、POS、MAX值。該圖片與文件中校正從動臂的內容相關，直觀呈現了校正過程中的參數情況。](../../en/images/d13-04.png)



## 注意事項

### ①有一個臂達到限位後不動了

需要重新校正

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ②找不到伺服馬達

![圖片展示的是Mac電腦終端機介面，顯示了Lerobot機器人相關程式碼執行時的錯誤資訊。錯誤提示FeetechMotorsBus馬達檢查在連接埠'/dev/tty.usbmodem5AAF2193061'失敗，缺少馬達ID - 1至 - 6，且預期模型為777。還列出全部預期馬達清單和全部找到的馬達清單。該圖片與文件中「常見Bug」部分相關，直觀呈現了找不到伺服馬達這一問題在程式碼執行時的錯誤表現。](../../en/images/d13-05.png)

伺服馬達電源沒插
