[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | 繁體中文 | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# 回看、回放資料集

## 視覺化整個資料集

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

輸入`TommyZihao/lerobot_zihao_dataset_a`，或者其他資料集

![圖片展示的是LeRobot Dataset Visualizer介面，畫面中有一個機器人，介面上方有「LeRobot Dataset Visualizer」字樣。介面中部有一個下拉式選單，顯示「TommyZihao/lerobot_zihao_dataset_a」等資料集選項，還有「Example Datasets」及其下的具體資料集名稱，下方有「Explore Open Datasets」藍色按鈕。該圖片與上文提到的視覺化整個資料集相關，對應輸入指定資料集的操作內容。](../../en/images/d29-01.png)

![圖片展示 addCriterion addCriterion](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![圖片展示的是LeroBot抓取柳橙資料集的視覺化介面。畫面中上方是抓取柳橙的影片，柳橙被夾在白色物體上。下方是資料圖表，顯示了多個變數隨時間變化的曲線，如「actuator」「gripper」「gripper_pos」等。左側有指令清單，目前選取「Grab Orangesanges」。右下角有播放、暫停等操作按鈕。該圖與上文提到的視覺化檢視指定episode內容相關，直觀呈現了抓取動作及對應資料。](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![圖片展示的是LeroBot資料集視覺化介面。左側為時間軸，可拖曳檢視不同時刻畫面。中間是攝影機畫面，顯示雙手在 自動產生](../../en/images/d29-04.png)
</column>
</grid>

觀察：指令和狀態是不一致的，指令由主動臂Leader提供，狀態由從動臂Follower提供

## 視覺化檢視指定episode

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![圖片展示的是neron.io平台中視覺化檢視指定episode的介面。左側為資料集結構，顯示有observation_images等資料。中間上方是即時攝影機畫面，畫面中有一個柳橙。右側是資料曲線圖，展示了不同資料隨時間的變化情況。底部是時間軸，可拖曳檢視任意時刻的資料。該圖片與上文「視覺化檢視指定episode」內容對應，直觀呈現了檢視指定episode時的介面及資料展示情況。](../../en/images/d29-05.png)

拖曳時間軸檢視任意時刻的攝影機畫面和伺服馬達位置

## 回放指定episode從動臂動作

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

能聽到聲音`Replaying episode`，然後從動臂移動，回放重現指定episode的動作

其實到這裡，已經能唬住很多外行了，不是嗎

<figure view-type="Preview">[附件 / Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
