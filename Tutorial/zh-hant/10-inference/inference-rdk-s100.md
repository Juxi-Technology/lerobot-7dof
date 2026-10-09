[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | 繁體中文 | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# 地瓜機器人 RDK S100推論

具體實現流程可以參考這個連結<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy 全流程文件" type="doc"></cite>



## 在 RDK S100/S100P 上進行 ACT 模型端到端部署

本節將帶你完成 ACT 模型在地瓜機器人 RDK S100 系列硬體的完整部署閉環。整個過程分為三個核心階段：**模型匯出**、**量化編譯** 和 **板端執行**。

<callout emoji="💡">
**前置說明：**
- **開發機 (Host)：**用於執行步驟 1 和步驟 2，通常為你的模型訓練機（需具備較好效能並安裝 Docker）。
- **板端 (Edge)：**地瓜機器人 RDK S100/S100P，用於執行步驟 3。
- **工具鏈：**本文相依於 `rdk_LeRobot_tools` 倉庫，詳情可參考 [GitHub 倉庫位址](https://github.com/D-Robotics/rdk_LeRobot_tools)。
</callout>

<callout emoji="🚨">
**版本相容性重要提示 (必讀)：** 目前版本的 `rdk_LeRobot_tools` ONNX 匯出流程完美相容 **LeRobot datasets v2.1** 版本。由於最新的 v3.0 版本存在資料結構修改，**強烈建議**在進行本章節操作前，將原始的 `lerobot` 主倉庫切換至相容 v2.1 的特定 commit，以確保匯出流程順暢。 
*推薦使用的 Commit ID：* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### 階段一：模型 ONNX 格式匯出 💻 (在開發機進行)

首先，我們需要將 **PyTorch 訓練好的** 模型匯出為中間格式（ONNX）。



#### **1. 拉取工具鏈倉庫** 

進入你的 `lerobot` 工作目錄，複製 RDK 專屬工具鏈：

```Bash
cd lerobot

# 1. 切换到兼容 v2.1 datasets 的稳定版本
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. 拉取地瓜机器人 RDK 专属工具链
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. 設定匯出參數** 

編輯 `rdk_LeRobot_tools/bpu_export_config.yaml` 檔案，根據你的實際路徑修改設定：

```YAML
dataset:
  root: "data/so101_pick_place" # 你的数据集绝对或相对地址
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # 原始 PyTorch 模型权重地址
type: "nash-e" # 目标硬件架构，RDK S100 对应 nash-e / S100P 对应 nash-m
```



#### 3. 執行匯出腳本

```Bash
# 导出 ONNX (开发机)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **成功標誌**：目前目錄下生成 `bpu_export_output` 資料夾，內部包含後續所需的 `build_all.sh` 腳本和量化標定資料。



### 階段二：編譯 BPU 模型 🐳 (在開發機 Docker 環境進行)

地瓜機器人的 BPU 模型量化與編譯需要相依於 OpenExplorer (OE) 環境。我們推薦使用 Docker 來隔離環境。



#### **1.** **準備 Docker 環境與映像檔** 

確保開發機已安裝 Docker（[官方安裝指南](https://docs.docker.com/engine/install/)）。下載推薦的 CPU 映像檔並載入：

```Bash
# 加载下载好的离线镜像压缩包
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. 啟動編譯容器**

<callout emoji="⚠️">
**避坑指南**：編譯模型需要較大的共享記憶體。請務必添加 `--shm-size=15g` 參數，否則極易引發 IPC 記憶體報錯。
</callout>

將開發機的工作目錄（包含剛才匯出的資料夾）掛載到容器內：

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(註：請將 `<docker-image-name>` 替換為你透過 `sudo docker images` 查看到的實際映像檔名。)



#### **3.** **容器內執行編譯** 

進入容器內部後，執行一鍵編譯腳本：

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **檢查編譯產物** 

編譯完成後，會在 `bpu_export_output` 下生成 `bpu_output/` 資料夾。這裡面包含了 RDK 板端執行所需的全部核心檔案： 

- 點擊查看 `bpu_output/` 目錄結構

  - `BPU_ACTPolicy_TransformerLayers.hbm` (量化後的模型檔案)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (量化後的模型檔案)
  - `action_mean.npy` 等若干資料集正規化參數
  - `camera1_mean.npy` 等攝影機統計參數

---

### 階段三：板端部署與推論 🤖 (在 RDK S100 上進行)

<callout emoji="📌">
**前置條件檢查：**
1. RDK 板端已設定好 `D-Robotics/lerobot` 執行環境，並安裝 `hbm_runtime`。
2. 已透過 `scp`、USB隨身碟等方式，將上一步生成的整個 `bpu_output/` 資料夾完整複製至 RDK 板端。
3. 已完成基礎的遙操作設定，確保機械臂序列埠、攝影機 USB 連接埠及標定檔案設定無誤。
</callout>



#### **1.** **執行 BPU 加速推論**

在 RDK 板端終端機，進入工具鏈目錄並啟動控制腳本：

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ 常見故障排查 (Troubleshooting)

在實際部署中如果遇到問題，請對照以下清單排查：

- **機械臂沒有動作？**

  - 檢查裝置掛載情況：終端機輸入 `ls /dev/ttyACM*`，確認機械臂對應的序列埠號是否正確。
  - 檢查權限：嘗試使用 `sudo` 執行推論腳本，或將目前使用者加入 `dialout` 使用者群組。
- **攝影機拉流報錯 / 畫面異常 / 機械臂原地抖動？**

  - 確認攝影機索引編號（Camera Index）是否因熱插拔發生漂移，檢查程式碼中的攝影機參數設定是否與實際 `/dev/video*` 對應。
- **開發機複製容器生成的檔案時提示「權限不夠」？**

  - Docker 掛載目錄產生的檔案歸屬預設為 root，在開發機執行 `sudo chown -R $USER:$USER bpu_export_output` 即可修復。
