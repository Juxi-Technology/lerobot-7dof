[English](../en/index.md) | 简体中文 | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>SO-ARM101机械臂 7轴 教程</title>

# SO-ARM101 7-DOF · 运行前准备

> 本说明面向把 **SO-ARM101 从 6 舵机改造成 7 舵机** 后，用 LeRobot 跑完整流程（标定 → 录制 → 训练 → 部署）的人。  
> 对应代码：本仓库（`lerobot-7dof`），它是对官方 lerobot 的 fork，仅改了 SO 相关电机配置。

---

## 0. 先确认你的机械臂

7 舵机（全部 STS3215），舵机 ID 与关节对应关系：

| **舵机 ID** | **关节名** | **说明** |
|-|-|-|
| 1 | `shoulder_pan` | 肩部水平旋转 |
| 2 | `shoulder_lift` | 肩部抬升 |
| 3 | `elbow_flex` | 肘部弯曲 |
| 4 | `wrist_flex` | 腕部俯仰（上下弯曲） |
| 5 | `wrist_yaw` | 腕部偏航（左右旋转约 90°）· **本次新增的舵机**（插在原 4、5 号之间） |
| 6 | `wrist_roll` | 腕部滚动 · 原 5 号滚动电机，ID 5→6，打印件未改、名字不变 |
| 7 | `gripper` | 夹爪 · 原 ID=6，改动后顺延到 7 |

关节数据顺序（录制后 Parquet 里 `action` / `observation.state` 的关节维顺序）：  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`。

⚠️ 注意：**6 舵机版本的数据、标定文件、已训练模型都与本仓库不兼容**，必须按下面重新做一遍。

---

## 1. 如果你是从官方代码仓库克隆来的，需要替换/修改哪些文件

### 方案 A：直接用本仓库代码（推荐）

<figure view-type="Card">[附件 / Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### 方案 B：在官方 lerobot git clone 后手动替换

从本仓库覆盖官方 clone 的**3 个文件**：

| 本仓库文件（源） | 覆盖到（目标） |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | 官方 clone 同名文件 |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | 官方 clone 同名文件 |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | 官方 clone 同名文件（**仅注释修正**，功能不影响，可不换） |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <官方clone>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <官方clone>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> 前提：你的官方 clone 与本仓库基线（lerobot 2026-08 版本）结构一致。若版本差距大，**不要整文件覆盖**，改为按下面“手动修改”两处即可。

### 版本不一致时的手动修改（只改两处）

**① 电机字典**（`so_follower.py` 和 `so_leader.py` 各一份，内容相同）——把原来的

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

**② 标定逻辑**（两文件各自的 `calibrate()`）——去掉“整圈关节”特判：把

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

替换为一行，改为记录全部关节的真实范围：

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> 为什么：原版把 `wrist_roll`（绕前臂轴滚动）当作可整圈（0\~4095）转动的关节硬编码全范围。7-DOF 改造后 5/6 号腕关节（yaw / roll）**都被机械限位、无法整圈**，硬设整圈会让代码把关节命令发到机械上到不了的角度，有损坏风险。现在标定时每个电机都手动记录实际 min/max。

### 双从臂 说明

`bi_so_follower` / `bi_so_leader`（`src/lerobot/robots/bi_so_follower/`、`src/lerobot/teleoperators/bi_so_leader/`）只是把单臂包一层加 `left_`/`right_` 前缀，**不含电机定义**。只要**上面的单臂文件**改好，双臂命令（`--robot.type=bi_so_follower`）自动就是 7-DOF。
