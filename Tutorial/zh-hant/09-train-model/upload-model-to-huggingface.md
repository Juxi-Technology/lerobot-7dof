[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | 繁體中文 | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# 上傳模型到HuggingFace（可選）

## 建立模型Repo

<grid>
<column width-ratio="0.354197">
![圖片展示了Hugging Face平台的使用者介面。畫面中有一個頭像圖示，點擊後彈出下拉式選單，其中「New Model」選項被紅色框線突出顯示。該圖片與文件中「上傳模型到HuggingFace（可選）」部分內容相關，對應「建立模型Repo」步驟，直觀呈現了在Hugging Face平台建立新模型的操作入口，幫助使用者了解如何在平台建立模型相關資源。](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![圖片展示的是Hugging Face網站上建立新模型倉庫的介面。介面中「Owner」下拉式選單已選擇「TommyZihao」，「Model name」輸入框為「lerobot_zihao_model_a」，「「License」輸入框內為「mit」。下方有「Base template」選項，以及「Public」和「Private」倉庫類型選擇。該圖片與文件中「建立模型Repo」部分相關，是建立模型倉庫時填寫相關資訊的示例介面。](../../en/images/d46-02.png)
</column>
</grid>

## 查看模型Repo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

現在是空的

![圖片展示的是Hugging Face平台上的「TommyZihao/lerobot_zihao_model_a」模型頁面。頁面左側有「Model card」選項卡，可編輯模型卡片。右側「Getting started with your model」部分介紹如何開始使用模型，包括新增完整模型資訊、推送模型檔案等。下方「Edit Model Card」區域可新增模型的License、語言、基礎模型等資訊。頁面底部有「Push your model files」區域，提供CLI、Python、Git、HTTPS、SSH等多種上傳模型檔案的方式。該圖片與文件中上傳模型到HuggingFace的內容相關，展示了模型頁面的操作介面。](../../en/images/d46-03.png)

![圖片展示的是Hugging Face平台上的TommyZihao/lerobot_zihao_model_a模型Repo頁面。頁面顯示該模型檔案大小為1.54 KB，有1個貢獻者，歷史記錄為1次提交，提交時間為9分鐘前。頁面還列出了.gitattributes和README.md檔案，它們的大小分別為1.52 KB和24 Bytes，同樣為初始提交，提交時間也是9分鐘前。該圖片與文件中上傳模型到HuggingFace的內容相關，展示了模型上傳後的頁面情況。](../../en/images/d46-04.png)

## 上傳模型

建立`upload_model.py`檔案，內容如下

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

執行

```Shell
python upload_model.py
```

![圖片展示了在命令列中執行`python upload_model.py`命令的輸出結果。顯示檔案處理進度為34%，新資料上傳進度同樣為34%，並列出了`d_model/model.safetensors`和`tokenizer_processor.safetensors`兩個檔案的上傳進度，分別為92%。該圖片與文件中上傳模型到HuggingFace的內容相關，直觀呈現了上傳模型檔案時的進度情況。](../../en/images/d46-05.png)

## 查看模型Repo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![圖片展示的是TommyZihao的lerobot_zihao_model_a模型在HuggingFace的Repo頁面。頁面顯示該模型的License為mit，有1個貢獻者，歷史記錄2次提交。頁面中部列出了多個檔案，如README.md、config.json等、model.safetensors等，每個檔案右側均有「Upload folder using huggingface_hub」字樣，表明這些檔案是透過huggingface_hub上傳的。該圖片與文件中上傳模型到HuggingFace的內容相關，直觀呈現了模型檔案在HuggingFace中的儲存情況。](../../en/images/d46-06.png)

現在有了模型檔案
