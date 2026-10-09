[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | 繁體中文 | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Windows電腦



<callout emoji="🚫">
需要同時接上主動臂和從動臂
</callout>

## 校正從動臂Follower

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![圖片展示的是在Windows電腦上使用命令列進行lerobot校正操作的介面。命令為「lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm」。介面中顯示了校正資訊，包括「zihao_follower_arm SO10IFollower connected」等提示，還列出了機械手臂各關節的NAME、MIN、POS、MAX值。校正過程中，提示使用者將機械手臂移動到其運動範圍的中間位置並按ENTER，同時記錄位置，按ENTER可停止。該圖片與文件中校正從動臂的內容相關，展示了具體操作步驟及介面回饋。](../../en/images/d14-01.png)

## 校正主動臂Leader

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![圖片展示的是在Windows電腦上使用lerobot-calibrate命令進行機械手臂校正的命令列介面。介面中顯示了校正從動臂和主動臂的相關資訊，包括校正位置儲存路徑、機器人類型、連接埠號、ID等。還提示將從動臂移到運動範圍中間並按ENTER，逐個關節通過其整個運動範圍，記錄位置，按ENTER停止。介面底部顯示了各關節的名稱、最小值、目前位置、最大值等資料。](../../en/images/d14-02.png)

## 檔案匯出位置

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## 更換機械手臂校正

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## 注意事項

### ①有一個臂達到限位後不動了

需要重新校正

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ②找不到伺服馬達

![圖片展示的是在macOS系統下執行lerobot程式時出現的錯誤資訊。程式執行時，出現「FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'」的RuntimeError，提示伺服馬達ID缺失，包括1 - 6號伺服馬達，預期模型號均為777，但實際找到的伺服馬達清單為空。這與文件中「找不到伺服馬達」的注意事項相關，可能是由於伺服馬達電源沒插，需重新插拔並轉一轉插孔。](../../en/images/d14-03.png)

伺服馬達電源沒插，重新插拔並轉一轉插孔
