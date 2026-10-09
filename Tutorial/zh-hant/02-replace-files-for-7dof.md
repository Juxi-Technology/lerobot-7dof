[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | 繁體中文 | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# 第二步：替換檔案（適配7DOF）

## 1. 如果你是從官方程式碼倉庫 clone 來的，需要替換/修改哪些檔案

### 方案 A：直接使用本倉庫程式碼（推薦）

<figure view-type="Card">[附件 / Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### 方案 B：在官方 lerobot git clone 後手動替換

從本倉庫覆蓋官方 clone 的**3 個檔案**：

| 本倉庫檔案（來源） | 覆蓋到（目標） |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | 官方 clone 同名檔案 |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | 官方 clone 同名檔案 |

<figure view-type="Card">[附件 / Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[附件 / Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> 前提：你的官方 clone 與本倉庫基線（lerobot 2026-09 版本）結構一致。
> 
> 若版本差距大，**不要整檔覆蓋**，改為按下面「手動修改」兩處即可。

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

`bi_so_follower` / `bi_so_leader`（`src/lerobot/robots/bi_so_follower/`、`src/lerobot/teleoperators/bi_so_leader/`）只是把單臂包一層加 `left_`/`right_` 前綴，**不含馬達定義**。只要**上面的單臂檔案**改好，雙臂命令（`--robot.type=bi_so_follower`）自動就是 7-DOF。
