[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | 繁體中文 | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# 示教採集資料集

## 刪除之前已經有的同名資料集（如果有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## 一個攝影機，採集資料集-Mac電腦

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## 兩個攝影機，採集資料集-Mac電腦

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=true \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## 採集中

<grid>
<column width-ratio="0.508765">
![圖片展示的是在Mac電腦上使用OpenVSLAM採集資料集時的終端機介面。介面上方顯示了採集參數，如解析度、影格率、編碼器等信息。下方是採集日誌，記錄了採集開始時間、版本資訊、執行緒數、編碼器等資料，還顯示了採集進度，如已採集298/298個episode，共5119.33秒。介面底部有「ESC」鍵操作說明，如立即停止並上傳資料集。該圖片與文件中採集資料集的操作流程相關，直觀呈現了採集過程中的終端機回饋資訊。](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![這張圖片展示了Mac電腦的命令列終端機介面，用於顯示攝影機採集資料集相關的執行日誌資訊。介面中包含SVT相關設定參數，如config參數、編碼程式庫版本資訊、各設定項的數值（如key frame、CRF、編碼解析度等），還能看到執行過程中的狀態日誌，例如MP4檔案處理的相關提示、裝置中斷連接的記錄、程式執行時的時間戳記資訊等，整體呈現了攝影機採集資料集過程中的後台執行狀態。](../../en/images/d26-02.png)
</column>
</grid>

鍵盤方向鍵操作：  
→（右箭頭）提前終止目前episode；進入下一個episode。  
←（左箭頭）取消目前episode；重新錄製。  
ESC，立即停止，編碼影片，並上傳資料集。

## 採集完畢，資料集儲存目錄

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```




## 握手

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.num_episodes=30 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```
