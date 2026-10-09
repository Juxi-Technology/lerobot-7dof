[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | 繁體中文 | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# 本機Ubuntu訓練

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- 注意

`\`前面只能有一個空格，後面不能有空格

`--dataset.split`預設為`train`，也就是用全量資料作為訓練集

資料集在本機，`--dataset.streaming`必須為`false`，因為資料集已經在本機，無需串流讀取

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![圖片展示了在Ubuntu本機環境下使用lerobot_train.py腳本進行訓練的命令及設定資訊。命令中包含資料集路徑、倉庫ID、分支等參數，如`--dataset.repo_id`為Tommy/lerobot_zhao_dataset_a等。設定資訊中，`--dataset.streaming`設為`false`，`--use_imagenet_stats`為`True`，`--batch_size`為4，`--val_n_episodes`為1000等。圖片與上下文緊密相關，直觀呈現了訓練時的命令及關鍵設定參數。](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![這張圖片展示了在本機Ubuntu環境下執行lerobot_train.py腳本時的訓練日誌內容，核心是訓練相關的設定參數與訓練過程的即時狀態資訊。圖片中明確標記了訓練的關鍵資訊，包括`--dataset.split`預設值為`train`，即採用全量資料作為訓練集，同時因資料集儲存於本機，對應`--dataset.streaming`的狀態明確。日誌內容還包含了訓練的進度資訊、資料集載入情況、模型最佳化器與排程器的建立資訊，以及訓練過程中的損耗、步數相關的數值資料，直觀呈現了本機訓練進行中的狀態。](../../en/images/d34-02.png)
</column>
</grid>

![圖片展示的是在Ubuntu環境下使用LeroBot訓練模型時的訓練日誌。日誌中記錄了訓練過程中的資訊，如時間、訓練集、模型、損失值、準確率等。其中，時間顯示為2024年1月14日15:11:53至16:15:16，損失值（loss）在0.68 - 0.65之間波動，準確率（acc）在0.85 - 0.88之間。該圖片與文件中LeroBot訓練模型的上下文相關，直觀呈現了訓練過程中的關鍵指標變化情況。](../../en/images/d34-03.png)
