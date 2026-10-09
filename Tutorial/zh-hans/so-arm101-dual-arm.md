[English](../en/so-arm101-dual-arm.md) | 简体中文 | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# SO-ARM101双从臂教程

## 简介

本指南介绍如何使用 LeRobot 训练双臂 SO-ARM 机器人系统的完整流程，包括硬件连接、双臂标定、双臂遥操作、数据集录制与管理、ACT 策略训练以及真实机器人部署。按照本指南操作，你可以使用两个主臂和两个从臂采集示教数据，训练模仿学习策略，并在真实机械臂上运行。

首先，按如下进行插线

| 角色 | 端口 |
|-|-|
| 左从臂 | /dev/ttyACM0 |
| 右从臂 | /dev/ttyACM1 |
| 左主臂 | /dev/ttyACM2 |
| 右主臂 | /dev/ttyACM3 |

从臂类型为 so101_follower，主臂类型为 so101_leader（LeRobot 中 so100_leader 和 so101_leader 共用同一实现）。

## 前置准备

### 0.1 安装依赖

环境安装请参考 SO-ARM 教程：

### 0.2 USB 权限

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## 标定（关键步骤）

### 1.1 标定左从臂

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 标定右从臂

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 标定左主臂

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 标定右主臂

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

标定完成后，文件会保存在：

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> 目录名说明：so101_follower 与 so100_follower、so101_leader 与 so100_leader 共用同一实现，因此目录统一为 so_follower / so_leader；主臂属于 teleoperator，标定文件在 teleoperators/ 下而不是 robots/。

### （可选）如果之前已用其他 ID 标定过

比如你之前用的是 my_awesome_follower_arm1、my_awesome_follower_arm2 等，可以复制校准文件：

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

## 双臂遥操作

### 2.1 不带摄像头

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

### 2.2 带摄像头

可用 lerobot-find-cameras opencv 查看摄像头索引，同时可自行添加或减少摄像头。

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

- 注意周围环境，避免从臂碰撞。

## 录制数据集

### 3.1 保存到本地（不上传 Hub）

添加 --dataset.root（数据写到该目录）和 --dataset.push_to_hub=false，并加 --dataset.no_stamp=true 保持数据集名稳定（否则 repo_id 会被自动追加时间戳，后续续录/回放/训练都会找不到它）。

> 注意：repo_id 建议包含 /（形如 用户名/数据集名），本地数据集不会真的上传。

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

> 视频编码默认已是 libsvtav1，无需指定；如需自定义，用 --dataset.rgb_encoder.vcodec=h264 这类嵌套参数。

数据会保存在 ./datasets/bi_so101_task/，结构为：

```Bash
├── meta/
│   ├── info.json         # 数据集信息（fps、特征形状等）
│   ├── episodes/         # 每集的元数据（chunk-000/...）
│   ├── stats.json        # 各特征归一化统计
│   └── tasks.parquet     # 任务文本 → task_index
├── data/                 # 每帧特征数据（chunk-*.parquet）
└── videos/               # 每个摄像头一个子目录（chunk-*.mp4）
```

### 3.2 上传到 Hugging Face Hub

如果你希望自动上传，保留 HF_USER 并去掉 root 和 push_to_hub=false（默认会上传）。端口和摄像头索引请与接线表保持一致：

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

> 上传后的 Hub 仓库名即为 ${HF_USER}/bi_so101_task，与下方 4.2 从 Hub 训练所用的 repo_id 一致。本地副本会先存到 ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/。

### 3.3 继续采集（断点续录）

如果录制过程中意外退出（例如按右键退出时处于 reset 阶段），或者想分多次完成采集，可以使用 --resume 继续往同一个数据集追加 episode。

**注意**：

- 必须加 --resume=true，否则 LeRobotDataset.create() 会因为目录已存在而报错。
- 续录命令的 --dataset.root 和 --dataset.repo_id 必须与首次录制（3.1）完全一致（resume 强制要求显式 root）。
- --dataset.num_episodes 是指**本次要录多少条**，不是总目标。例如已录 15 条，想凑够 50 条，就写 35。
- 退出时尽量在 episode 录制过程中或自然结束后再退出，避免在 "Reset the environment" 阶段退出（会导致空 episode 保存失败）。

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

### 3.4 回放与删除 episode

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

> episode 是 0-based 索引，24 表示第 25 条 episode。

#### 删除指定 episode

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

删除后会原地重写数据集，原数据会备份到 ./datasets/bi_so101_task_old/。确认新数据集无误后，可以手动删除备份：

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### 删除整条数据集

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## ACT 训练

### 4.1 从本地数据集训练

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

> --dataset.root 指向 3.1 录制的数据集目录（repo_id 要与录制时一致）。若 --output_dir 目录已存在，会直接报 FileExistsError，请换一个新的输出目录或加 --resume=true 续训。

### 4.2 从 Hugging Face Hub 训练

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

> 上面使用了 ACT 的默认参数（chunk_size=100、dim_model=512 等）。
> 
> repo_id 须与 3.2 上传时的仓库名一致（3.2 已加 --dataset.no_stamp=true，仓库名固定为 \${HF_USER}/bi_so101_task）。训练时无需 --dataset.root，会自动从 Hub 下载。

## 真实机器人部署

> 注意：lerobot-record 只用于采集示教数据。部署训练好的策略请用 lerobot-rollout——当前版本 lerobot-record 不再接受 --policy.path，也会拒绝 eval\_ 前缀的数据集名。

### 5.1 现场评估（不录制数据）

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

- --duration 为运行秒数，0 表示不限时。
- 如需中途接管/停止，加 --interactive=true，在终端用 /stop、/reset 等命令控制。

### 5.2 评估并录制数据（本地）

用 episodic 策略（行为类似旧版 lerobot-record，按 episode 录制并带 reset 阶段）：

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

> 部署数据集名必须以 rollout\_ 开头（当前版本的强制约定）。录制到本地时建议加 --dataset.root 和 --dataset.no_stamp=true，避免目录名被追加时间戳。

### 5.3 评估数据上传到 Hugging Face Hub

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

## 常见问题

| 问题 | 原因 | 解决方案 |
|-|-|-|
| 遥操时提示重新标定 | bi_so_follower 找不到 \_left / \_right 后缀的校准文件 | 用带_left / \_right 的 ID 重新标定，或复制已有校准文件 |
| 主臂无法拖动 | leader 扭矩未关闭 | 重新标定或检查电机 |
| 继续采集时报目录已存在 | 未加--resume=true | 在lerobot-record 命令中添加 --resume=true |
| --resume=true 时报错要求 root | 续录必须显式指定数据集目录 | 续录命令添加--dataset.root=./datasets/bi_so101_task，且与首次录制保持一致 |
| 数据集目录名多出时间戳，回放/训练找不到 | 录制时未设置no_stamp，repo_id 被自动追加时间戳 | 录制/续录时添加--dataset.no_stamp=true |
| --dataset.vcodec=... 报参数不存在 | 旧版参数，当前视频编码参数已改为嵌套 | 改用--dataset.rgb_encoder.vcodec=h264（默认已是libsvtav1） |
| 部署时 lerobot-record 报 --policy.path / eval\_ 错误 | 当前版本 lerobot-record 已不含策略部署能力 | 部署改用lerobot-rollout --strategy.type=episodic，数据集名以rollout_开头 |
| 左右臂反了 | 端口配置错误 | 交换left_arm_config.port 和 right_arm_config.port |
| 训练时找不到数据集 | 本地数据集未指定root | 训练时添加--dataset.root=./datasets/xxx |
| 数据集被自动上传 | 未设置push_to_hub=false | 录制时添加--dataset.push_to_hub=false |
| 退出时报You must add one or several frames before calling add_episode | 在 reset 阶段退出，当前 episode 没有帧 | 不影响已录数据，用--resume=true 继续采集 |