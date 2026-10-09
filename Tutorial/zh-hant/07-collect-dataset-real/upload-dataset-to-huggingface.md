[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | 繁體中文 | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>上傳資料集到HuggingFace（選用）</title>

# 方法一：本機上傳（不建議，上傳網速慢）

- 自動上傳

在採集資料集時設定`push_to_hub=true`，採集完畢後自動上傳

![這張圖片展示了命令列執行介面，屬於資料集上傳相關流程的執行日誌內容，介面上方顯示了SVN、treet W2等工具及環境相關資訊，中間標註了「Starting the second pass: moving the mov atom to the beginning of the file」這樣的處理提示，下方記錄了執行過程中的錯誤提示，如「error messaging the mach port for IMCRunLoopWakeUpReliable」，同時右側列出了處理進度和資料傳輸的相關數值，比如不同條目對應的資料量、速度等資訊，整體呈現出資料集上傳處理過程中的執行狀態記錄。](../../en/images/d28-01.png)

- 手動上傳

在採集資料集時設定`push_to_hub=false`，採集完畢後手動上傳

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



無論自動上傳還是手動上傳，上傳速度都很慢（每秒鐘一百KB）

因為HuggingFace伺服器在國外

# 方法二：雲GPU平台上傳（建議）

## 登入雲GPU平台Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## 開啟一個雲GPU執行個體

## 上傳資料集壓縮檔到`資料集`

## 複製執行個體下載命令

![圖片展示的是Featurize平台中資料集頁面。頁面上方顯示「資料集」標題，下方是名為「soarm_amazing_hand_pick.zip」的資料集，其大小為213.3 MB，上傳時間為16小時前。頁面右側有「雲解壓縮」按鈕，以及「喜歡」「評論」「複製執行個體下載命令」等操作按鈕。該圖片與文件中「上傳資料集壓縮檔到`資料集`」的操作步驟相關，展示了上傳後的資料集頁面情況。](../../en/images/d28-02.png)

## 在雲GPU執行個體的命令列中執行

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## 上傳資料集到HuggingFace

建立`upload_dataset.py`檔案，內容如下

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

執行檔案

```Shell
python upload_dataset.py
```

![這張圖片展示了在雲GPU執行個體命令列中執行上傳資料集操作的過程，顯示名為lerobot2的使用者執行了python upload.py命令。介面中呈現了處理檔案的進度資訊，共需處理6個檔案，所有檔案的處理進度均達到100%，還標註了各檔案的傳輸大小，總資料傳輸進度為100%。底部提示沒有檔案自上次提交以來被修改，因此跳過提交以防止建立空提交，該內容對應文件中執行upload_dataset.py檔案的步驟場景。](../../en/images/d28-03.png)

- 另一種上傳方法（不建議）

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![這張圖片展示了使用HuggingFace的`hf upload`命令上傳資料集的過程，命令為`hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`。圖片中顯示上傳已進入最終階段，目前所有檔案的處理進度均為100%，其中包含多個影片格式檔案與parquet格式檔案，各自的上傳體積也與對應本機檔案大小完全相符，同步顯示了上傳的檔案總大小與傳輸速度，底部還附帶了此次上傳提交的HuggingFace資料集頁面連結，體現了上傳任務已完成的狀態。](../../en/images/d28-04.png)

# 檢視HuggingFace上的資料集

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![這張圖片是Hugging Face平台上Juxi-Technology團隊的soarm_amazing_hand_pick資料集詳情頁面擷圖，屬於文件中檢視HuggingFace上的資料集對應的內容。頁面頂部顯示了該資料集的相關導覽選項，以及資料集的作者、標籤等基本資訊，中間的Dataset Viewer區域展示了該資料集split 1的訓練資料部分內容，包含action、observation_state、timestamp等資料欄位及對應數值，還標註了單條資料的大小、總資料條數和總大小等資訊，頁面下方還提及了基於該資料訓練的相關模型。](../../en/images/d28-05.png)

![圖片展示的是Hugging Face平台上的soarm_amazing_hand_pick資料集頁面。頁面上方有搜尋框及導覽列，可搜尋模型、資料集等。資料集資訊部分顯示所屬組織為Juxi - Technology，包含標籤如robotics、imitation-learning等。下方「Files and versions」標籤下，列出data、meta、videos等資料夾及README.md檔案，顯示上傳者、上傳方式、時間等資訊，如「Upload README.md with huggingface_hub」等。該圖與文件中檢視HuggingFace資料集的內容相關，直觀呈現了資料集的檔案及版本情況。](../../en/images/d28-06.png)
