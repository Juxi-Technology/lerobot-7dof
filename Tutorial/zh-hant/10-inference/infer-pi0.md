[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | 繁體中文 | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# 推論命令列-pi0

## Ubuntu

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- 推論命令列

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![圖片展示的是在Ubuntu系統下使用SSH連接到機器時出現的錯誤資訊。畫面中顯示了主控台輸出的程式碼錯誤，指出平台不被支援，無法取得X連接，還提示需確保有執行的X伺服器，且DISPLAY環境變數設定正確。此外，還顯示了關於無頭環境的警告資訊，以及錄製第0集的記錄。該圖片與文件中Ubuntu推論命令列操作上下文相關，可能是操作過程中遇到的異常情況。](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![圖片展示的是在Ubuntu系統下執行推論命令列時的輸出結果。命令執行過程中，出現多次「E0119」錯誤提示，指出在自動調校時無有效triton設定，且out of resources，如shared memory資源不足。還顯示了多個triton_msm模型的執行參數，如ALLOW_TF32、BLOCK_K、BLOCK_M等，以及相應的ACC_TYPE、ALLOW_TF32、BLOCK_K、BLOCK_M等資訊。該圖片與文件中Ubuntu推論命令列操作上下文相關，展示了執行過程中遇到的資源不足問題。](../../en/images/d51-02.png)
</column>
</grid>

![圖片展示的是在Ubuntu系統下推論命令列操作的終端機介面。介面中顯示了多個triton_mm指令的執行結果，如triton_mm_3644耗時0.2355ms等，均採用t1.float32類型，ALLOW_TF32=True，BLOCK_K等參數也有所顯示。最後顯示SingleProcess AUTOTUNE benchmarking耗時0.7305秒和0.0001秒預編譯20個選擇。該圖片與文件中Ubuntu推論命令列操作上下文相關，展示了具體執行情況。](../../en/images/d51-03.png)

> **影片待補**：原文此處嵌有 `VID_20260120_182109.mp4`（原始 310MB）。飛書側該檔案未提供可下載的影片串流，只有中繼資料，因此未能抓取。需查看請到[原文件](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg)。



## Mac

- 刪除原有的eval開頭的資料集（如有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論命令列

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![圖片展示的是在Ubuntu系統下使用11 - yolo26推論命令列時的終端機介面。介面中顯示了Python 3.12版本資訊，以及對robot - type設定為follower的記錄。還列出了攝影機相關參數，如color_mode、fourcc、fps、height、width等。此外，還顯示了載入模型的路徑，以及一些警告資訊，如模型載入錯誤等。該圖片與文件中Ubuntu推論命令列操作上下文相關，直觀呈現了操作過程中的終端機回饋資訊。](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![圖片展示的是在Ubuntu系統中使用Python程式碼進行推論時的命令列輸出。輸出中包含多個資訊，如「PIBPytorch model」載入成功，「WARNING」提示可能需要處理的模型鍵，「INFO」顯示OpenCV攝影機連接成功等。此外，還出現了多次「huggingface/tokenizers: The process current just got forked...」的警告資訊，提示因fork操作導致的並行問題。該圖片與上下文介紹的Ubuntu系統推論命令列操作相關，展示了實際執行時可能出現的各類資訊及警告。](../../en/images/d51-05.png)
</column>
</grid>

## 用Mac推論，機械臂一頓一頓的原因

- 資料集太小了
- 顯示卡顯示記憶體不夠，需要上50系顯示卡
