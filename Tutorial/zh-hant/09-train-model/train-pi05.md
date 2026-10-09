[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | 繁體中文 | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# 訓練命令列-pi0.5

## 參考文件

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## 推薦雲GPU實例

![圖片展示的是阿里雲提供的RTX A6000雲GPU實例資訊。其按量使用價格為每小時3.29元，有3卡可用。實例設定包括RTX A6000顯示卡，共51.0 GB顯示記憶體；30核AMD EPYC 7742 CPU；60.9 GB記憶體；429.5 GB硬碟。底部有一個藍色的「開始使用」按鈕。該圖片與文件中「推薦雲GPU實例」部分內容相關，直觀呈現了推薦的雲GPU實例設定及價格等關鍵資訊。](../../en/images/d40-01.png)

## 安裝環境

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## 命令列

- 刪除之前訓練中斷的output下的檔案

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- 訓練

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi05 \
    --output_dir=~/output_lerobot_train/shake/pi05_A \
    --job_name=shake_pi05_A \
    --policy.pretrained_path=lerobot/pi05_base \
    --policy.compile_model=true \
    --policy.gradient_checkpointing=true \
    --policy.dtype=bfloat16 \
    --policy.freeze_vision_encoder=false \
    --policy.train_expert_only=false \
    --steps=50000 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project \
    --batch_size=8
```

<grid>
<column width-ratio="0.468128">
![圖片展示的是在命令列中訓練物理智慧代理（PI）的輸出資訊。畫面中顯示了模型載入、參數重映射、optimizer和scheduler建立等過程，如「Loading model from: lerobot/pi05_base」等。還出現了「Warning: Could not remap state dicts: \[『loading』\] in state_dict for PolicyPolicy」等警告資訊。此外，還呈現了訓練相關資料，如「num_total_frames: 180K」等。該圖片與文件中訓練命令列操作內容相關，直觀呈現了訓練過程中的關鍵資訊。](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![圖片展示的是在命令列訓練時的輸出資訊。訓練過程中，huggingface/torch的行程被fork，因已使用並行，故停用並行以避免死鎖。同時，提示避免使用「before the fork if possible」等資訊。在訓練20分鐘後，訓練才會正式開始。圖片與上下文緊密相關，直觀呈現了訓練過程中可能出現的行程操作及提示資訊，幫助理解訓練過程中的狀態及注意事項。](../../en/images/d40-02.png)
</column>
</grid>

命令列執行20分鐘後，訓練才會正式開始

模型壓縮檔5個G左右，解壓縮後7個G
