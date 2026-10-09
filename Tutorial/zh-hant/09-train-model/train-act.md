[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | 繁體中文 | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# 訓練命令列-ACT（推薦入門）

## 參考文件

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## 為什麼從ACT演算法開始

ACT是玩LeRobot最推薦訓練的第一個模型，它的好處如下：

- 模型非常輕量，只有八千萬個可學習參數
- 訓練收斂速度很快，推論速度也很快
- 在單卡GPU上訓練一個小時就能看到效果
- ACT模型下載壓縮檔大概200MB左右，非常便於儲存和傳輸
- 資料集採集30輪資料基本就夠用了
- 可以部署在Ubuntu主機、Mac電腦、Windows電腦，甚至樹莓派上推論
- 真實機器人推論效果還很不錯，對於夾取、握手、放筆這類簡單任務足夠了
- LeRobot庫的基礎環境中已經自帶了ACT演算法，無需安裝其它庫

## 命令列

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## 命令列說明

換行符`\`前面只能有一個空格，後面不能有空格

紅色為每次執行之前都要檢查或者修改的參數

| 命令列參數 | 說明 |
|-|-|
| --dataset.repo_id | HuggingFace資料集的Repo_ID |
| --dataset.root | 資料集本機路徑 |
| --dataset.revision | 資料集版本，在上傳資料集到HuggingFace的時候指定過的 |
| --dataset.streaming | 資料集在本機，必須為`false`，因為資料集已經在本機，無需串流讀取 |
| --dataset.split | 預設為`train`，也就是用全量資料作為訓練集 |
| --policy.type | 要訓練的演算法，比如act、smolvla、diffusion、pi0、wallx |
| --output_dir | 輸出結構儲存的目錄 |
| --job_name | 本次訓練任務的名字 |
| --policy.device | 運算裝置 |
| --wandb.enable | 開啟wandb視覺化 |
| --wandb.project | wandb專案名稱 |
| --policy.push_to_hub | 將訓練好的模型發到HuggingFace雲端 |
| --steps | 訓練步數 |
| --batch_size | 一步輸入的資料量，如果顯示記憶體不夠，應該調小 |
|  |  |

## 訓練過程

<grid>
<column width-ratio="0.357753">
![圖片展示的是在命令列中使用`lerobot-train`訓練命令的示例。命令列中包含多個參數設定，如`--dataset.repo_id`、`--dataset.root`等，用於指定資料集相關資訊。還設定了`--policy.type`為`act`，`--output_dir`為`outputs/lerobot_train/output_a`等輸出目錄，以及`--job_name`、`--policy.device`等其他參數。此外，還列出了`--dataset.split`、`--policy.push_to_hub`等參數的預設值。圖片與上下文緊密相關，直觀呈現了訓練命令列中各參數的設定情況。](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![圖片展示的是訓練過程中的命令列輸出內容。顯示了模型訓練的相關資訊，如scheduler、steps、use_policy_training_preset等參數設定，以及資料集相關參數。還呈現了模型參數數量、損失函式等資訊，如num_total_params為55917096（52M），loss為0.626等。下方有檔案下載資訊，如下載「https://download.pytorch.org/models/resnet18-f37072fd.pth」到/home/featurize/.cache/torch/hub/checkpoints目錄。該圖片與上下文介紹的訓練命令列相關，直觀呈現了訓練中訓練命令列輸出的內容。](../../en/images/d36-02.png)
</column>
</grid>

![圖片展示的是訓練過程中的日誌資訊。日誌以時間順序記錄了訓練的多個步驟，包括時間、訓練輪數、損失值、學習率等資料。如2024年1月14日15:11:53的訓練輪數為131k，損失值為0.368等。其中，`INFO`標識為日誌類型，`train`表示訓練階段，`step`為訓練輪數，`loss`為損失值，`lr`為學習率。該圖片與文件中訓練命令列說明部分相關，直觀呈現了訓練過程中的關鍵資料。](../../en/images/d36-03.png)

模型壓縮檔大概300MB
