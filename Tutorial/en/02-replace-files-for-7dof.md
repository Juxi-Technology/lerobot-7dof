English | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# Step 2: Replace Files (for 7-DOF)

## 1. If You Cloned the Official Code Repository, Which Files Need to Be Replaced/Modified

### Option A: Use This Repository's Code Directly (Recommended)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Option B: Manually Replace After Cloning the Official lerobot

Copy **3 files** from this repository over your official clone:

| This repository's file (source) | Overwrite to (target) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | The same-named file in the official clone |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | The same-named file in the official clone |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> Prerequisite: your official clone has the same structure as this repository's baseline (the lerobot 2026-09 release).
> 
> If the versions differ a lot, **do not overwrite whole files**, instead make the two "manual modification" changes below.

### Manual Modification When the Versions Differ (Only Two Places)

**① Motor dictionary** (one copy each in `so_follower.py` and `so_leader.py`, with identical content) — change

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

to

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # newly added servo, left/right rotation
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # the original servo-5 roll motor, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② Calibration logic** (the `calibrate()` in each of the two files) — remove the "full-turn joint" special case: change

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

to a single line that records the real range of every joint:

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> Why: the original code hard-codes `wrist_roll` (rolling around the forearm axis) as a joint that can turn a full circle (0\~4095) and sets its range to the full span. After the 7-DOF conversion, the two wrist joints (yaw / roll, servos 5/6) are **both mechanically limited and cannot turn a full circle**; forcing a full-circle range makes the code send joint commands to angles the mechanics cannot reach, with a risk of damage. Now, during calibration, every motor's actual min/max is recorded manually.

### Note on the Dual Follower Arms

`bi_so_follower` / `bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) merely wrap a single arm and add a `left_`/`right_` prefix; they **contain no motor definitions**. As long as the **single-arm files above** are done, the dual-arm commands (`--robot.type=bi_so_follower`) are automatically 7-DOF.
