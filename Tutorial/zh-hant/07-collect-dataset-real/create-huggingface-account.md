[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | 繁體中文 | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# 註冊Hugging Face帳號（選用）

## 設定HuggingFace國內鏡像

- Ubuntu

```Shell
sudo nano ~/.bashrc

# 在文件末尾加入
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# 输出
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# 在文件末尾加入
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# 输出
# https://hf-mirror.com
```



## 建立Token

https://huggingface.co/settings/tokens

![圖片展示的是Hugging Face網站中「設定權杖」頁面。頁面左側有使用者頭像及個人資訊，右側是「使用者存取權杖」區域，說明存取權杖用於身分驗證並允許應用程式基於權杖權限執行操作，強調不與他人分享權杖。頁面右上角有「+ Create new token」按鈕，用於建立新權杖。該圖片與文件中「建立Token」部分內容相關，直觀呈現了建立Token的操作入口。](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![這張圖片展示了Hugging Face平台中建立新存取憑證（Token）的頁面。頁面中「Token type」區域醒目提示了「Write」選項，同時「Token name」輸入框中已填寫內容「lerobot_zihao_token」，頁面底部還有用於提交建立操作的「Create token」按鈕，頁面左側顯示目前登入使用者的帳號資訊。](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![圖片展示的是Hugging Face網站上「Save your Access Token」彈出視窗介面。彈窗提示使用者將token值保存在安全的地方，關閉後無法再次檢視。下方顯示token值「<your-huggingface-token>」，並有「Copy」按鈕用於複製token。下方還有token名稱「lerobot_zihao_token」及權限「WRITE」。該圖片與文件中「建立Token」步驟相關，用於指導使用者記錄產生的Token。](../../en/images/d24-03.png)
</column>
</grid>

## 記錄Token

例如，我的是：

```Shell
<your-huggingface-token>
```

## 綁定Token

```Shell
hf auth login

hf auth whoami
```

![圖片展示了在Mac系統中使用命令列操作Hugging Face帳號的介面。首先執行「hf auth login」命令，提示已有token，可使用「hf auth whoami」取得資訊或「hf auth logout」登出。接著輸入token，確認後顯示token已儲存至指定路徑，且目前作用中的token為「lerobot zihao token」，最後執行「hf auth whoami」命令，顯示使用者為「TommyZihao」。該圖片與文件中「建立Token」部分對應，直觀呈現了建立Token的操作及結果。](../../en/images/d24-04.png)

## 建立Dataset Repo

<grid>
<column width-ratio="0.480604">
![這是Hugging Face平台的介面截圖，頁面左上角有Community、Docs、Pricing等功能入口，個人帳號TommyZihao的資訊在介面中間區域，被綠色框標註的是頁面右上角的使用者頭像圖示。介面中間選單中，用紅色框醒目標註了「+ New Dataset」選項，該選項對應上下文中「建立Dataset Repo」的操作步驟，點擊此選項即可開始建立資料集程式庫。](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![圖片展示的是在Hugging Face建立新資料集儲存庫的介面。介面中「Dataset name」處輸入了「lerobot_zihao_dataset_a」，「License」選擇為「mit」，「Public」選項被勾選。底部有「Create dataset」按鈕。該圖片與文件中「建立Dataset Repo」部分內容對應，直觀呈現了建立資料集儲存庫時需填寫的關鍵資訊，協助使用者瞭解在Hugging Face上建立資料集儲存庫的操作介面及所需填寫的欄位。](../../en/images/d24-06.png)
</column>
</grid>

![這是Hugging Face平台的網頁介面，頁面展示了使用者TommyZhao建立的、名為lexobot_zihao_dataset_a的Dataset儲存庫內容，處於Dark主題模式下。頁面左側有「Edit Dataset Card」的編輯選項，還提供了推送資料集檔案的多種操作方式，包含CLI、Python、HTTPS、SSH的相關命令範例，同時標註了該資料集的License為mit。](../../en/images/d24-07.png)
