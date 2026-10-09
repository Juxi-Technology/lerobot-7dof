English | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>SO-ARM101 7-DOF Tutorial</title>

# SO-ARM101 7-DOF · Preparation Before Running

> This guide is for people who have **converted an SO-ARM101 from 6 servos to 7 servos** and now want to run the full LeRobot workflow (calibration → recording → training → deployment) on it.  
> Corresponding code: this repository (`lerobot-7dof`), a fork of the official lerobot that changes only the SO-related motor configuration.

---

## 0. First, Confirm Your Robotic Arm

7 servos (all STS3215). The mapping between servo IDs and joints:

| **Servo ID** | **Joint name** | **Description** |
|-|-|-|
| 1 | `shoulder_pan` | Shoulder horizontal rotation |
| 2 | `shoulder_lift` | Shoulder lift |
| 3 | `elbow_flex` | Elbow bend |
| 4 | `wrist_flex` | Wrist pitch (up/down bend) |
| 5 | `wrist_yaw` | Wrist yaw (left/right rotation of about 90°) · **the newly added servo this time** (inserted between the original servos 4 and 5) |
| 6 | `wrist_roll` | Wrist roll · originally the servo-5 roll motor, ID 5→6; the printed part is unchanged and the name stays the same |
| 7 | `gripper` | Gripper · originally ID=6, shifted to 7 after the change |

Joint data order (the order of the joint dimensions of `action` / `observation.state` in the Parquet files after recording):  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`.

⚠️ Note: **the data, calibration files and already-trained models of the 6-servo version are incompatible with this repository**, so you must redo everything below from scratch.

---

## 1. If You Cloned the Official Code Repository, Which Files Need to Be Replaced/Modified

### Option A: Use This Repository's Code Directly (Recommended)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Option B: Manually Replace After Cloning the Official lerobot

Copy **3 files** from this repository over your official clone:

| This repository's file (source) | Overwrite to (target) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | The same-named file in the official clone |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | The same-named file in the official clone |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | The same-named file in the official clone (**comments only**, no functional impact; optional) |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <official-clone>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <official-clone>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> Prerequisite: your official clone has the same structure as this repository's baseline (the lerobot 2026-08 release). If the versions differ a lot, **do not overwrite whole files** — instead make the two "manual modification" changes below.

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
