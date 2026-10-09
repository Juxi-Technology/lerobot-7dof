[English](../en/02-replace-files-for-7dof.md) | 简体中文 | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# 第二步：替换文件（适配7DOF）

## 1. 如果你是从官方代码仓库克隆来的，需要替换/修改哪些文件

### 方案 A：直接用本仓库代码（推荐）

<figure view-type="Card">[附件 / Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### 方案 B：在官方 lerobot git clone 后手动替换

从本仓库覆盖官方 clone 的**3 个文件**：

| 本仓库文件（源） | 覆盖到（目标） |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | 官方 clone 同名文件 |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | 官方 clone 同名文件 |

<figure view-type="Card">[附件 / Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[附件 / Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> 前提：你的官方 clone 与本仓库基线（lerobot 2026-09 版本）结构一致。
> 
> 若版本差距大，**不要整个文件覆盖**，改为按下面“手动修改”两处即可。

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
