[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | 日本語 | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# コマンドラインリファレンス

## コマンドラインの注意点

ライブ可視化あり： --display_data=true

ライブ可視化なし： --display_data=false

`--display_data=true` にすると、クールな rerun.io の可視化インターフェースが起動しますが、ディレクトリ `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` に 1 フレームごとに画像が保存され、多くの容量を消費します。後で `--display_data=false` に設定してかまいません。



HuggingFace のモデルリポジトリからモデルを推論する： --policy.path=Tommymy/lerobot_my_model_a



## 把持オレンジタスクを例にする

- ローカルモデルを推論する（ライブ可視化あり）

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- ローカルモデルを推論する（ライブ可視化なし）

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- HuggingFace のモデルリポジトリからモデルを推論する

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --policy.path=Tommymy/lerobot_my_model_a
```

モデルは実行後にダウンロードされます

![この画像は、コマンドラインで `pretrained_model.py` スクリプトを実行するインターフェースを示しています。上部にはモデルのパラメーター構成として、`--display_data=true` や `--policy.path=TommyZihao/lerobot_zihao_model_a` などが表示されています。その下には「robot」「camera」「calibration_dir」などのパラメーター設定があります。最下部にはモデルのダウンロード進捗が表示され、現在は 68% です。この画像は、`pretrained_model.py` スクリプトの実行とそのパラメーターの説明に関連し、パラメーター設定とダウンロード進捗を視覚的に示しています。](../../en/images/d48-01.png)
