[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | 繁體中文 | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Ubuntu電腦

## 為連接埠賦予權限

讓所有使用者都有權限讀寫這些序列埠裝置

```Shell
sudo chmod 666 /dev/ttyACM*
```

## 校正從動臂Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![圖片展示的是Ubuntu電腦上執行「lerobot-calibrate」命令校正從動臂Follower的終端機介面。介面中顯示了從動臂的連接資訊、關節名稱、上下限值等資料。關鍵資訊有：按Enter開始校正，依序轉動每個關節達到上下限位，按Enter結束校正；以及「Calibration saved to」等校正檔案路徑資訊。該圖片與文件中校正從動臂Follower的操作步驟緊密相關，直觀呈現了校正過程中的終端機回饋。](../../en/images/d12-01.png)

## 校正Leader主動臂

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![圖片展示的是Ubuntu電腦為連接埠賦予權限後的介面。命令列中輸入「sudo chmod 666 /dev/ttyACM*」，執行後顯示「zihao_leader_arm」等資訊。下方有「按Enter開始校正」「依序轉動每個關節達到每個關節的上下限位」「按Enter結束校正」等提示，以及「Calibration saved to」等校正相關路徑資訊。該圖片與上文「校正Leader主動臂」內容對應，直觀呈現了校正前的準備工作介面。](../../en/images/d12-02.png)

## 檢視校正設定檔

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![圖片展示的是Ubuntu電腦終端機介面中顯示的「zihao_follower_arm.json」檔案內容。檔案包含多個臂的設定資訊，如shoulder_pan、shoulder_lift、elbow_flex、wrist_flex等，每個臂有id、drive_mode、homing_offset、range_min、range_max等參數。該圖片與上文「檢視校正設定檔」內容相關，直觀呈現了校正設定檔中的具體參數資訊，協助使用者瞭解各臂的設定情況。](../../en/images/d12-03.png)



## 注意事項

### ①有一個臂達到限位後不動了

需要重新校正

<figure view-type="Preview">[附件 / Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ②找不到伺服馬達

![這是一張顯示在Ubuntu系統終端機中的報錯介面擷圖，對應文件中「找不到伺服馬達」的注意事項說明。介面提示發生了RuntimeError錯誤，具體為「FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'」，即伺服馬達檢查失敗。介面還列出了預期的伺服馬達資訊，預期的馬達ID為1-6，對應模型均為777，但最終找到的馬達清單為空，結合上下文可知該報錯是因伺服馬達未通電所導致的。](../../en/images/d12-04.png)

伺服馬達電源沒插
