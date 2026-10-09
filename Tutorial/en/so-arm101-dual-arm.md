English | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# SO-ARM101 Dual-Arm Tutorial

## Introduction

This guide walks through the complete workflow for training a dual-arm SO-ARM robot system with LeRobot, including hardware wiring, dual-arm calibration, dual-arm teleoperation, dataset recording and management, ACT policy training, and real-robot deployment. Following this guide, you can use two leader arms and two follower arms to collect demonstration data, train an imitation-learning policy, and run it on the real arms.

First, wire everything up as follows

| Role | Port |
|-|-|
| Left follower | /dev/ttyACM0 |
| Right follower | /dev/ttyACM1 |
| Left leader | /dev/ttyACM2 |
| Right leader | /dev/ttyACM3 |

The follower type is so101_follower and the leader type is so101_leader (in LeRobot, so100_leader and so101_leader share the same implementation).

## Prerequisites

### 0.1 Install dependencies

For environment setup, refer to the SO-ARM tutorial:

### 0.2 USB permissions

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## Calibration (Critical Step)

### 1.1 Calibrate the left follower

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 Calibrate the right follower

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 Calibrate the left leader

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 Calibrate the right leader

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

After calibration, the files are saved to:

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> Note on directory names: so101_follower and so100_follower, and so101_leader and so100_leader, share the same implementation, so the directories are unified as so_follower / so_leader. The leader is a teleoperator, so its calibration files live under teleoperators/ rather than robots/.

### (Optional) If you previously calibrated with other IDs

For example, if you previously used my_awesome_follower_arm1, my_awesome_follower_arm2, etc., you can copy the calibration files:

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

## Dual-Arm Teleoperation

### 2.1 Without cameras

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

### 2.2 With cameras

You can use lerobot-find-cameras opencv to check camera indices, and add or remove cameras as you like.

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

### Safety tips

- Watch the surroundings and avoid collisions between the follower arms.

## Recording a Dataset

### 3.1 Save locally (without uploading to the Hub)

Add --dataset.root (the directory the data is written to) and --dataset.push_to_hub=false, and add --dataset.no_stamp=true to keep the dataset name stable (otherwise a timestamp is automatically appended to the repo_id, and later resume/playback/training will not find it).

> Note: the repo_id should contain / (in the form username/dataset-name); a local dataset is not actually uploaded.

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

> Video encoding is already libsvtav1 by default, so it needs no specifying; to customize, use a nested parameter such as --dataset.rgb_encoder.vcodec=h264.

The data is saved under ./datasets/bi_so101_task/, with this structure:

```Bash
├── meta/
│   ├── info.json         # Dataset info (fps, feature shapes, etc.)
│   ├── episodes/         # Per-episode metadata (chunk-000/...)
│   ├── stats.json        # Normalization stats for each feature
│   └── tasks.parquet     # Task text → task_index
├── data/                 # Per-frame feature data (chunk-*.parquet)
└── videos/               # One subdirectory per camera (chunk-*.mp4)
```

### 3.2 Upload to the Hugging Face Hub

If you want automatic upload, keep HF_USER and remove root and push_to_hub=false (upload is the default). Keep the ports and camera indices consistent with the wiring table:

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

> The uploaded Hub repository name is ${HF_USER}/bi_so101_task, matching the repo_id used for Hub-based training in 4.2 below. A local copy is first saved to ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/.

### 3.3 Continue recording (resume)

If recording exited unexpectedly (for example, you exited with a right-click while in the reset phase), or you want to finish collection over several sessions, use --resume to keep appending episodes to the same dataset.

**Note**:

- You must add --resume=true, otherwise LeRobotDataset.create() errors out because the directory already exists.
- In the resume command, --dataset.root and --dataset.repo_id must exactly match the first recording (3.1) (resume requires an explicit root).
- --dataset.num_episodes is **how many episodes to record this time**, not the total target. For example, if you already recorded 15 and want 50 in total, write 35.
- When exiting, try to quit during an episode recording or right after it ends naturally; avoid exiting during the "Reset the environment" phase (it causes an empty episode to fail to save).

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

### 3.4 Playback and deleting episodes

#### Play back a specific episode

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

> episode is a 0-based index, so 24 means the 25th episode.

#### Delete a specific episode

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

Deleting rewrites the dataset in place, and the original data is backed up to ./datasets/bi_so101_task_old/. Once you have confirmed the new dataset is correct, you can manually remove the backup:

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### Delete the entire dataset

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## ACT Training

### 4.1 Train from a local dataset

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

> --dataset.root points to the dataset directory recorded in 3.1 (the repo_id must match the one used when recording). If the --output_dir directory already exists, it raises FileExistsError immediately — use a new output directory or add --resume=true to continue training.

### 4.2 Train from the Hugging Face Hub

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

> The command above uses ACT's default parameters (chunk_size=100, dim_model=512, etc.).
> 
> The repo_id must match the repository name used when uploading in 3.2 (3.2 adds --dataset.no_stamp=true, so the repository name is fixed as \${HF_USER}/bi_so101_task). No --dataset.root is needed for training; it is downloaded from the Hub automatically.

## Real-Robot Deployment

> Note: lerobot-record is only for collecting demonstration data. Use lerobot-rollout to deploy a trained policy — the current version of lerobot-record no longer accepts --policy.path and also rejects dataset names with the eval\_ prefix.

### 5.1 On-site evaluation (no data recorded)

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

- --duration is the number of seconds to run; 0 means no time limit.
- To take over/stop mid-run, add --interactive=true and use commands such as /stop and /reset in the terminal.

### 5.2 Evaluate and record data (locally)

Use the episodic strategy (behaves like the old lerobot-record: records by episode with a reset phase):

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

> A deployment dataset name must start with rollout\_ (a hard requirement of the current version). When recording locally, add --dataset.root and --dataset.no_stamp=true to avoid a timestamp being appended to the directory name.

### 5.3 Upload evaluation data to the Hugging Face Hub

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

## FAQ

| Issue | Cause | Solution |
|-|-|-|
| Teleoperation asks to recalibrate | bi_so_follower cannot find calibration files with the \_left / \_right suffix | Recalibrate with IDs that include _left / \_right, or copy existing calibration files |
| The leader arm cannot be dragged | Leader torque is not disabled | Recalibrate or check the motor |
| Resume recording reports the directory already exists | --resume=true was not added | Add --resume=true to the lerobot-record command |
| --resume=true errors and demands a root | Resume requires an explicit dataset directory | Add --dataset.root=./datasets/bi_so101_task to the resume command, matching the first recording |
| The dataset directory name has an extra timestamp, so playback/training cannot find it | no_stamp was not set when recording, so a timestamp was appended to the repo_id | Add --dataset.no_stamp=true when recording/resuming |
| --dataset.vcodec=... reports that the parameter does not exist | It is an old parameter; the video encoding parameter is now nested | Use --dataset.rgb_encoder.vcodec=h264 instead (the default is already libsvtav1) |
| During deployment, lerobot-record reports a --policy.path / eval\_ error | The current version of lerobot-record no longer includes policy deployment | Use lerobot-rollout --strategy.type=episodic for deployment, with dataset names starting with rollout_ |
| The left and right arms are swapped | Wrong port configuration | Swap left_arm_config.port and right_arm_config.port |
| Training cannot find the dataset | No root was specified for the local dataset | Add --dataset.root=./datasets/xxx when training |
| The dataset is uploaded automatically | push_to_hub=false was not set | Add --dataset.push_to_hub=false when recording |
| On exit, it reports You must add one or several frames before calling add_episode | You exited during the reset phase, so the current episode has no frames | Does not affect already-recorded data; use --resume=true to continue collecting |
