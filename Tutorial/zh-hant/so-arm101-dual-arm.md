[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | 繁體中文 | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# SO-ARM101雙從臂教學

## 簡介

本指南介紹如何使用 LeRobot 訓練雙臂 SO-ARM 機器人系統的完整流程，包括硬體連接、雙臂標定、雙臂遙操作、資料集錄製與管理、ACT 策略訓練以及真實機器人部署。按照本指南操作，你可以使用兩個主臂和兩個從臂採集示教資料，訓練模仿學習策略，並在真實機械手臂上執行。

首先，按如下進行插線

| 角色 | 連接埠 |
|-|-|
| 左從臂 | /dev/ttyACM0 |
| 右從臂 | /dev/ttyACM1 |
| 左主臂 | /dev/ttyACM2 |
| 右主臂 | /dev/ttyACM3 |

從臂類型為 so101_follower，主臂類型為 so101_leader（LeRobot 中 so100_leader 和 so101_leader 共用同一實現）。

## 前置準備

### 0.1 安裝相依套件

環境安裝請參考 SO-ARM 教學：

### 0.2 USB 權限

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## 標定（關鍵步驟）

### 1.1 標定左從臂

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 標定右從臂

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 標定左主臂

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 標定右主臂

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

標定完成後，檔案會儲存在：

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> 目錄名說明：so101_follower 與 so100_follower、so101_leader 與 so100_leader 共用同一實現，因此目錄統一為 so_follower / so_leader；主臂屬於 teleoperator，標定檔案在 teleoperators/ 下而不是 robots/。

### （選用）如果之前已用其他 ID 標定過

比如你之前用的是 my_awesome_follower_arm1、my_awesome_follower_arm2 等，可以複製校正檔案：

```Bash
CAL_DIR=~/.cache/huggingface/lerobot/calibration

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm1.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_left.json

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm2.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_right.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm3.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_left.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm4.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_right.json
```

---

## 雙臂遙操作

### 2.1 不帶攝影機

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### 2.2 帶攝影機

可用 lerobot-find-cameras opencv 查看攝影機索引，同時可自行新增或減少攝影機。

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### 安全提示

- 注意周圍環境，避免從臂碰撞。

## 錄製資料集

### 3.1 儲存到本機（不上傳 Hub）

新增 --dataset.root（資料寫到該目錄）和 --dataset.push_to_hub=false，並加 --dataset.no_stamp=true 保持資料集名穩定（否則 repo_id 會被自動附加時間戳，後續續錄/回放/訓練都會找不到它）。

> 注意：repo_id 建議包含 /（形如 使用者名稱/資料集名），本機資料集不會真的上傳。

```Bash
lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> 影片編碼預設已是 libsvtav1，無需指定；如需自訂，用 --dataset.rgb_encoder.vcodec=h264 這類巢狀參數。

資料會儲存在 ./datasets/bi_so101_task/，結構為：

```Bash
├── meta/
│   ├── info.json         # 数据集信息（fps、特征形状等）
│   ├── episodes/         # 每集的元数据（chunk-000/...）
│   ├── stats.json        # 各特征归一化统计
│   └── tasks.parquet     # 任务文本 → task_index
├── data/                 # 每帧特征数据（chunk-*.parquet）
└── videos/               # 每个摄像头一个子目录（chunk-*.mp4）
```

### 3.2 上傳到 Hugging Face Hub

如果你希望自動上傳，保留 HF_USER 並去掉 root 和 push_to_hub=false（預設會上傳）。連接埠和攝影機索引請與接線表保持一致：

```Bash
export HF_USER=your_hf_username

lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> 上傳後的 Hub 倉庫名即為 ${HF_USER}/bi_so101_task，與下方 4.2 從 Hub 訓練所用的 repo_id 一致。本機副本會先存到 ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/。

### 3.3 繼續採集（中斷點續錄）

如果錄製過程中意外退出（例如按右鍵退出時處於 reset 階段），或者想分多次完成採集，可以使用 --resume 繼續往同一個資料集附加 episode。

**注意**：

- 必須加 --resume=true，否則 LeRobotDataset.create() 會因為目錄已存在而報錯。
- 續錄命令的 --dataset.root 和 --dataset.repo_id 必須與首次錄製（3.1）完全一致（resume 強制要求顯式 root）。
- --dataset.num_episodes 是指**本次要錄多少條**，不是總目標。例如已錄 15 條，想湊夠 50 條，就寫 35。
- 退出時盡量在 episode 錄製過程中或自然結束後再退出，避免在 "Reset the environment" 階段退出（會導致空 episode 儲存失敗）。

```Bash
lerobot-record \
  --resume=true \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=35 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

### 3.4 回放與刪除 episode

#### 回放指定 episode

```Bash
lerobot-replay \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.episode=24
```

> episode 是 0-based 索引，24 表示第 25 條 episode。

#### 刪除指定 episode

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

刪除後會原地重寫資料集，原資料會備份到 ./datasets/bi_so101_task_old/。確認新資料集無誤後，可以手動刪除備份：

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### 刪除整條資料集

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## ACT 訓練

### 4.1 從本機資料集訓練

```Bash
lerobot-train \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=60000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> --dataset.root 指向 3.1 錄製的資料集目錄（repo_id 要與錄製時一致）。若 --output_dir 目錄已存在，會直接報 FileExistsError，請換一個新的輸出目錄或加 --resume=true 續訓。

### 4.2 從 Hugging Face Hub 訓練

```Bash
export HF_USER=your_hf_username

lerobot-train \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=100000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> 上面使用了 ACT 的預設參數（chunk_size=100、dim_model=512 等）。
> 
> repo_id 須與 3.2 上傳時的倉庫名一致（3.2 已加 --dataset.no_stamp=true，倉庫名固定為 \${HF_USER}/bi_so101_task）。訓練時無需 --dataset.root，會自動從 Hub 下載。

## 真實機器人部署

> 注意：lerobot-record 只用於採集示教資料。部署訓練好的策略請用 lerobot-rollout——目前版本 lerobot-record 不再接受 --policy.path，也會拒絕 eval\_ 前綴的資料集名。

### 5.1 現場評估（不錄製資料）

```Bash
lerobot-rollout \
  --strategy.type=base \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --task="Pick the cube with left arm and hand it to right arm" \
  --duration=60 \
  --display_data=true
```

- --duration 為執行秒數，0 表示不限時。
- 如需中途接管/停止，加 --interactive=true，在終端機用 /stop、/reset 等命令控制。

### 5.2 評估並錄製資料（本機）

用 episodic 策略（行為類似舊版 lerobot-record，按 episode 錄製並帶 reset 階段）：

```Bash
lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=juxi/rollout_bi_so101_task \
  --dataset.root=./datasets/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

> 部署資料集名必須以 rollout\_ 開頭（目前版本的強制約定）。錄製到本機時建議加 --dataset.root 和 --dataset.no_stamp=true，避免目錄名被附加時間戳。

### 5.3 評估資料上傳到 Hugging Face Hub

```Bash
export HF_USER=your_hf_username

lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=${HF_USER}/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

## 常見問題

| 問題 | 原因 | 解決方案 |
|-|-|-|
| 遙操時提示重新標定 | bi_so_follower 找不到 \_left / \_right 後綴的校正檔案 | 用帶_left / \_right 的 ID 重新標定，或複製已有校正檔案 |
| 主臂無法拖動 | leader 扭矩未關閉 | 重新標定或檢查馬達 |
| 繼續採集時報目錄已存在 | 未加--resume=true | 在lerobot-record 命令中新增 --resume=true |
| --resume=true 時報錯要求 root | 續錄必須顯式指定資料集目錄 | 續錄命令新增--dataset.root=./datasets/bi_so101_task，且與首次錄製保持一致 |
| 資料集目錄名多出時間戳，回放/訓練找不到 | 錄製時未設定no_stamp，repo_id 被自動附加時間戳 | 錄製/續錄時新增--dataset.no_stamp=true |
| --dataset.vcodec=... 報參數不存在 | 舊版參數，目前影片編碼參數已改為巢狀 | 改用--dataset.rgb_encoder.vcodec=h264（預設已是libsvtav1） |
| 部署時 lerobot-record 報 --policy.path / eval\_ 錯誤 | 目前版本 lerobot-record 已不含策略部署能力 | 部署改用lerobot-rollout --strategy.type=episodic，資料集名以rollout_開頭 |
| 左右臂相反 | 連接埠配置錯誤 | 交換left_arm_config.port 和 right_arm_config.port |
| 訓練時找不到資料集 | 本機資料集未指定root | 訓練時新增--dataset.root=./datasets/xxx |
| 資料集被自動上傳 | 未設定push_to_hub=false | 錄製時新增--dataset.push_to_hub=false |
| 退出時報You must add one or several frames before calling add_episode | 在 reset 階段退出，目前 episode 沒有幀 | 不影響已錄資料，用--resume=true 繼續採集 |