[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | 繁體中文 | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# 常見Bug及解決

## 攝影機取得失敗

![圖片展示的是LeroBot機器人相關程式碼執行時的終端機輸出資訊。其中，`INFO`日誌顯示了OpenCV攝影機開啟、Follower斷連等資訊；`ERROR`日誌則指出在`camera_opencv.py`檔案中，`read`函式因`OpenCVCamera(0) read failed`而引發`RuntimeError`。該圖片與文件中「攝影機取得失敗」問題相關，直觀呈現了程式碼執行時出現的錯誤情況，協助理解該問題在實際運行中的表現。](../../en/images/d55-01.png)

檢查一下腕部攝影機的接線是否鬆動，特別是靠近攝影機那端的接線，非常容易接觸不良

## 攝影機斷連

![圖片展示的是LeroBot HF在運行過程中出現的攝影機取得失敗問題的程式碼及錯誤資訊介面。介面上方顯示了OpenCV攝影機連接失敗的提示，下方是`lerobot_record.py`檔案中`record_loop`函式的錯誤堆疊，指出在`async_read`時發生`TimeoutError`，提示攝影機在200ms內未取得影格。該圖片與文件中「攝影機取得失敗」問題的上下文對應，直觀呈現了該問題在程式碼層面的具體表現。](../../en/images/d55-02.png)

重新啟動一下命令列

## 伺服馬達通訊問題1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![這張圖片是一段終端機報錯日誌，對應文件中提及的伺服馬達通訊問題1的相關內容。日誌中顯示了具體的程式碼檔案路徑，在`lerobot/src/lerobot/motors/motors_bus.py`檔案的第1069行、第1181行，記錄了`ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`的錯誤資訊，還標註了`There is no status packet!`的錯誤原因，完整呈現了文件中提到的伺服馬達通訊問題1的報錯詳情。](../../en/images/fix-04.png)

解決方案，把`lerobot/src/lerobot/motors/motors_bus.py`程式碼中所有`num_retry`都改成99，特別是報錯行對應的

![圖片展示的是LeroBot專案中`motors_bus.py`程式碼檔案內容。檔案中`MotorsBusABC`類別的`write`方法被醒目提示，其中`num_retry`變數被修改為`99`。該圖片與文件中「伺服馬達通訊問題1」部分相關，對應解決方案中提到的把`lerobot/src/lerobot/motors/motors_bus.py`程式碼中所有`num_retry`都改成99的操作，特別是報錯行對應的程式碼部分。](../../en/images/d55-03.png)

## 伺服馬達通訊問題2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![這張圖片展示了一份錯誤日誌，屬於文件中「伺服馬達通訊問題2」對應的報錯內容，具體是在執行程式碼時出現了名為ConnectionError的錯誤，具體表現為Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries，附帶的錯誤資訊還說明There is no status packet，同時還標註了觸發該錯誤的程式碼所在路徑，這份錯誤日誌與文件中「伺服馬達通訊問題2」的描述完全對應，是該問題的實際報錯記錄。](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![圖片展示的是在macOS系統下，使用zsh終端機執行命令列操作的介面。終端機顯示了多個檔案路徑及程式碼檔案內容，其中`/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py`檔案中存在`ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`的報錯資訊。該圖片與文件中「伺服馬達通訊問題2」內容相關，直觀呈現了報錯時的終端機介面情況。](../../en/images/fix-05.png)
</column>
</grid>

解決方法：重新標定機械臂
