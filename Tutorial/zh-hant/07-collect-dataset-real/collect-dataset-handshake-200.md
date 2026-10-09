[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | 繁體中文 | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# 示教採集資料集-握手200

## 在HuggingFace上建立Dataset Repo

https://huggingface.co/new-dataset

![圖片展示了在HuggingFace上建立新資料集儲存庫的介面。介面中「Owner」處顯示為TommyZihao，「Dataset name」輸入框內填寫為lerobot_zihao_dataset_shake200，「License」選擇為mit，「Public」選項被選中，下方說明任何人都可檢視此資料集，只有資料集擁有者或組織成員可提交。底部提示資料集建立後，可使用網頁介面或git上傳檔案，且有「Create dataset」按鈕。該圖片與文件中在HuggingFace上建立Dataset Repo的內容相關，是建立資料集儲存庫操作的介面展示。](../../en/images/d27-01.png)

## 刪除之前已經有的同名資料集（如果有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Shake200資料集採集

一個攝影機，採集資料集-Mac電腦

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
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake200 \
    --dataset.num_episodes=200 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```

## 採集中

<grid>
<column width-ratio="0.508765">
![這是一段用於採集握手資料集（Shake200）時的螢幕錄影終端機介面內容，介面頂部和中部偏下均有標註為「INFO 202-04-15 17:00:22.227 Recording episode 36」的資訊，顯示正在錄製第36個資料採集週期（episode），同時呈現了SVT-AV1編碼器的相關參數，以及進度資訊「200/200 \[00:00:00::00, 4911.09 examples/s\]」，體現該階段正處於資料集採集過程中，與文件描述的「採集中」環節對應，展示了採集時的系統運行狀態。](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![這張圖片是Mac終端機的運行介面，顯示了SVT專案的設定與運行日誌資訊。介面中包含多項設定參數，如SVT設定的 GOP 大小、mini-pod 大小、關鍵影格類型、CRF 等數值，還有SVT-A1 Encoder程式庫的版本資訊。介面下半部分的日誌記錄了影片檔案操作相關的內容，提到移動原子到檔案開頭，以及多個OpenVCamera、SV280 follower等元件的連接與斷開狀態，最終顯示目前路徑下的終端機執行結束提示，對應示教採集握手200資料集過程中的終端機運行情境。](../../en/images/d27-02.png)
</column>
</grid>

鍵盤方向鍵操作：  
→（右箭頭）提前終止目前episode；進入下一個episode。  
←（左箭頭）取消目前episode；重新錄製。  
ESC，立即停止，編碼影片，並上傳資料集。

## 採集完畢，資料集儲存目錄

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
