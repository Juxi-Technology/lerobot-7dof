[English](../en/index.md) | [简体中文](../zh-hans/index.md) | 繁體中文 | [Deutsch](../de/index.md) | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>SO-ARM101機械臂 7軸 教程</title>

# SO-ARM101 7-DOF · 運行前準備

> 本說明適合把 **SO-ARM101 從 6 伺服馬達改造成 7 伺服馬達**後，用 LeRobot 跑完整流程（校正 → 錄製 → 訓練 → 部署）的人。  
> 對應程式碼：本倉庫（`lerobot-7dof`），它是官方 lerobot 的 fork，僅改了 SO 相關馬達設定。

---

## 0. 先確認你的機械臂

7 伺服馬達（全部 STS3215），伺服馬達 ID 與關節對應關係：

| **伺服馬達 ID** | **關節名** | **說明** |
|-|-|-|
| 1 | `shoulder_pan` | 肩部水平旋轉 |
| 2 | `shoulder_lift` | 肩部抬升 |
| 3 | `elbow_flex` | 肘部彎曲 |
| 4 | `wrist_flex` | 腕部俯仰（上下彎曲） |
| 5 | `wrist_yaw` | 腕部偏航（左右旋轉約 90°）· **本次新增的伺服馬達**（插在原 4、5 號之間） |
| 6 | `wrist_roll` | 腕部滾動 · 原 5 號滾動馬達，ID 5→6，列印件未改、名稱不變 |
| 7 | `gripper` | 夾爪 · 原 ID=6，改動後順延到 7 |

關節資料順序（錄製後 Parquet 裡 `action` / `observation.state` 的關節維順序）：  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`。

⚠️ 注意：**6 伺服馬達版本的資料、校正檔案、已訓練模型都與本倉庫不相容**，必須按下面重新做一遍。

---

## 1. 如果你是從官方程式碼倉庫 clone 來的，需要替換/修改哪些檔案

### 方案 A：直接使用本倉庫程式碼（推薦）

<figure view-type="Card">[附件 / Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### 方案 B：在官方 lerobot git clone 後手動替換

從本倉庫覆蓋官方 clone 的**3 個檔案**：

| 本倉庫檔案（來源） | 覆蓋到（目標） |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | 官方 clone 同名檔案 |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | 官方 clone 同名檔案 |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | 官方 clone 同名檔案（**僅註解修正**，功能不影響，可不換） |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <官方clone>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <官方clone>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> 前提：你的官方 clone 與本倉庫基線（lerobot 2026-08 版本）結構一致。若版本差距大，**不要整檔覆蓋**，改為按下面「手動修改」兩處即可。

### 版本不一致時的手動修改（只改兩處）

**① 馬達字典**（`so_follower.py` 和 `so_leader.py` 各一份，內容相同）——把原來的

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

改成

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # 新增舵机，左右旋转
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # 原 5 号滚动电机，ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② 校正邏輯**（兩檔案各自的 `calibrate()`）——去掉「整圈關節」特判：把

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

替換為一行，改為記錄全部關節的真實範圍：

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> 為什麼：原版把 `wrist_roll`（繞前臂軸滾動）當作可整圈（0\~4095）轉動的關節硬編碼全範圍。7-DOF 改造後 5/6 號腕關節（yaw / roll）**都被機械限位、無法整圈**，硬設整圈會讓程式碼把關節命令送到機械上到不了的角度，有損壞風險。現在校正時每個馬達都手動記錄實際 min/max。

### 雙從動臂 說明

`bi_so_follower / bi_so_leader`（`src/lerobot/robots/bi_so_follower/`、`src/lerobot/teleoperators/bi_so_leader/`）只是把單臂包一層加 `left_`/`right_` 前綴，**不含馬達定義**。只要**上面的單臂檔案**改好，雙臂命令（`--robot.type=bi_so_follower`）自動就是 7-DOF。
