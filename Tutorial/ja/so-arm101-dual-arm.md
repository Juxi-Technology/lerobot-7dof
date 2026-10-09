[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | 日本語 | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# SO-ARM101 デュアルアーム チュートリアル

## はじめに

このガイドでは、LeRobot を使ってデュアルアームの SO-ARM ロボットシステムを学習するための完全なワークフローを順を追って説明します。ハードウェアの配線、デュアルアームのキャリブレーション、デュアルアームのテレオペレーション、データセットの記録と管理、ACT ポリシーの学習、実機へのデプロイを含みます。このガイドに従えば、2本の Leader アームと2本の Follower アームを使って実演データを収集し、模倣学習ポリシーを学習し、実機のアームで実行できます。

まず、次のようにすべてを配線します

| 役割 | ポート |
|-|-|
| 左 Follower | /dev/ttyACM0 |
| 右 Follower | /dev/ttyACM1 |
| 左 Leader | /dev/ttyACM2 |
| 右 Leader | /dev/ttyACM3 |

Follower のタイプは so101_follower、Leader のタイプは so101_leader です（LeRobot では、so100_leader と so101_leader は同じ実装を共有しています）。

## 前提条件

### 0.1 依存関係のインストール

環境構築については、SO-ARM チュートリアルを参照してください：

### 0.2 USB の権限

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## キャリブレーション（重要なステップ）

### 1.1 左 Follower のキャリブレーション

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 右 Follower のキャリブレーション

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 左 Leader のキャリブレーション

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 右 Leader のキャリブレーション

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

キャリブレーション後、ファイルは以下に保存されます：

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> ディレクトリ名についての注意：so101_follower と so100_follower、および so101_leader と so100_leader は同じ実装を共有しているため、ディレクトリは so_follower / so_leader に統一されています。Leader はテレオペレータであるため、そのキャリブレーションファイルは robots/ ではなく teleoperators/ の下にあります。

### （オプション）以前に別の ID でキャリブレーションしていた場合

たとえば、以前に my_awesome_follower_arm1、my_awesome_follower_arm2 などを使用していた場合、キャリブレーションファイルをコピーできます：

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

## デュアルアームのテレオペレーション

### 2.1 カメラなし

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

### 2.2 カメラあり

lerobot-find-cameras opencv を使ってカメラのインデックスを確認でき、カメラは自由に追加・削除できます。

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

### 安全上のヒント

- 周囲に注意し、Follower アーム同士の衝突を避けてください。

## データセットの記録

### 3.1 ローカルに保存する（Hub にアップロードしない）

--dataset.root（データを書き込むディレクトリ）と --dataset.push_to_hub=false を追加し、さらに --dataset.no_stamp=true を追加してデータセット名を安定させます（そうしないと repo_id にタイムスタンプが自動で付加され、後の再開・再生・学習で見つからなくなります）。

> 注意：repo_id には / を含める必要があります（username/dataset-name の形式）。ローカルデータセットは実際にはアップロードされません。

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

> 動画エンコードはデフォルトで既に libsvtav1 のため、指定する必要はありません。カスタマイズするには、--dataset.rgb_encoder.vcodec=h264 のようなネストされたパラメータを使用します。

データは ./datasets/bi_so101_task/ の下に保存され、次の構成になります：

```Bash
├── meta/
│   ├── info.json         # Dataset info (fps, feature shapes, etc.)
│   ├── episodes/         # Per-episode metadata (chunk-000/...)
│   ├── stats.json        # Normalization stats for each feature
│   └── tasks.parquet     # Task text → task_index
├── data/                 # Per-frame feature data (chunk-*.parquet)
└── videos/               # One subdirectory per camera (chunk-*.mp4)
```

### 3.2 Hugging Face Hub にアップロードする

自動アップロードを行いたい場合は、HF_USER を設定したまま、root と push_to_hub=false を削除します（アップロードがデフォルトです）。ポートとカメラのインデックスは配線表と一致させてください：

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

> アップロードされる Hub リポジトリ名は ${HF_USER}/bi_so101_task で、後述の 4.2 で Hub ベースの学習に使用する repo_id と一致します。ローカルコピーはまず ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/ に保存されます。

### 3.3 記録を続ける（再開）

記録が予期せず終了した場合（たとえばリセット段階で右クリックで終了した場合）、または収集を複数回のセッションに分けて完了させたい場合は、--resume を使って同じデータセットにエピソードを追加し続けます。

**注意**：

- --resume=true を追加する必要があります。そうしないと、ディレクトリが既に存在するため LeRobotDataset.create() がエラーになります。
- 再開コマンドでは、--dataset.root と --dataset.repo_id を最初の記録（3.1）と正確に一致させる必要があります（再開には明示的な root が必要です）。
- --dataset.num_episodes は**今回記録するエピソード数**であり、目標の合計数ではありません。たとえば、すでに 15 記録していて合計 50 にしたい場合は、35 と書きます。
- 終了するときは、エピソードの記録中か、それが自然に終了した直後に終了するようにしてください。「環境をリセットする」段階での終了は避けてください（空のエピソードが保存できず失敗します）。

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

### 3.4 再生とエピソードの削除

#### 特定のエピソードを再生する

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

> episode は 0 始まりのインデックスなので、24 は 25 番目のエピソードを意味します。

#### 特定のエピソードを削除する

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

削除するとデータセットがその場で書き換えられ、元のデータは ./datasets/bi_so101_task_old/ にバックアップされます。新しいデータセットが正しいことを確認したら、バックアップを手動で削除できます：

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### データセット全体を削除する

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## ACT 学習

### 4.1 ローカルデータセットから学習する

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

> --dataset.root は 3.1 で記録したデータセットディレクトリを指します（repo_id は記録時に使用したものと一致させる必要があります）。--output_dir ディレクトリが既に存在する場合は、直ちに FileExistsError が発生します — 新しい出力ディレクトリを使用するか、--resume=true を追加して学習を続けてください。

### 4.2 Hugging Face Hub から学習する

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

> 上記のコマンドは ACT のデフォルトパラメータ（chunk_size=100、dim_model=512 など）を使用しています。
> 
> repo_id は 3.2 でアップロードした際のリポジトリ名と一致させる必要があります（3.2 では --dataset.no_stamp=true を追加するため、リポジトリ名は \${HF_USER}/bi_so101_task に固定されます）。学習に --dataset.root は不要です。Hub から自動的にダウンロードされます。

## 実機へのデプロイ

> 注意：lerobot-record は実演データの収集専用です。学習済みポリシーをデプロイするには lerobot-rollout を使用してください — 現在のバージョンの lerobot-record は --policy.path を受け付けなくなり、また eval\_ プレフィックスの付いたデータセット名も拒否します。

### 5.1 現場での評価（データを記録しない）

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

- --duration は実行する秒数です。0 は時間制限なしを意味します。
- 実行中に引き継ぎ / 停止するには、--interactive=true を追加し、ターミナルで /stop や /reset などのコマンドを使用します。

### 5.2 評価してデータを記録する（ローカル）

episodic ストラテジーを使用します（以前の lerobot-record と同様に動作します。リセット段階を挟んでエピソード単位で記録します）：

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

> デプロイ用データセット名は rollout\_ で始まる必要があります（現在のバージョンのハード要件です）。ローカルで記録する場合は、ディレクトリ名にタイムスタンプが付加されるのを避けるため、--dataset.root と --dataset.no_stamp=true を追加してください。

### 5.3 評価データを Hugging Face Hub にアップロードする

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

| 問題 | 原因 | 解決策 |
|-|-|-|
| テレオペレーションで再キャリブレーションを求められる | bi_so_follower が \_left / \_right サフィックスの付いたキャリブレーションファイルを見つけられない | _left / \_right を含む ID で再キャリブレーションするか、既存のキャリブレーションファイルをコピーします |
| Leader アームをドラッグできない | Leader のトルクが無効化されていない | 再キャリブレーションするか、モータを確認します |
| 記録の再開でディレクトリが既に存在すると報告される | --resume=true が追加されていない | lerobot-record コマンドに --resume=true を追加します |
| --resume=true がエラーになり root を要求する | 再開には明示的なデータセットディレクトリが必要 | 再開コマンドに --dataset.root=./datasets/bi_so101_task を追加し、最初の記録と一致させます |
| データセットのディレクトリ名に余分なタイムスタンプが付き、再生 / 学習で見つからない | 記録時に no_stamp を設定していないため、repo_id にタイムスタンプが付加された | 記録 / 再開時に --dataset.no_stamp=true を追加します |
| --dataset.vcodec=... がパラメータが存在しないと報告する | 古いパラメータであり、動画エンコードのパラメータはネストされるようになった | 代わりに --dataset.rgb_encoder.vcodec=h264 を使用します（デフォルトは既に libsvtav1） |
| デプロイ中に lerobot-record が --policy.path / eval\_ のエラーを報告する | 現在のバージョンの lerobot-record にはポリシーのデプロイが含まれなくなった | デプロイには lerobot-rollout --strategy.type=episodic を使用し、データセット名は rollout_ で始めます |
| 左右のアームが入れ替わっている | ポート設定が誤っている | left_arm_config.port と right_arm_config.port を入れ替えます |
| 学習でデータセットが見つからない | ローカルデータセットの root を指定していない | 学習時に --dataset.root=./datasets/xxx を追加します |
| データセットが自動でアップロードされる | push_to_hub=false を設定していない | 記録時に --dataset.push_to_hub=false を追加します |
| 終了時に You must add one or several frames before calling add_episode と報告される | リセット段階で終了したため、現在のエピソードにフレームがない | すでに記録済みのデータには影響しません。--resume=true で収集を続けてください |
