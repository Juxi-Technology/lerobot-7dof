[English](../en/10-inference/infer-pi05.md) | [简体中文](../zh-hans/10-inference/infer-pi05.md) | [繁體中文](../zh-hant/10-inference/infer-pi05.md) | [Deutsch](../de/10-inference/infer-pi05.md) | [Español](../es/10-inference/infer-pi05.md) | [Français](../fr/10-inference/infer-pi05.md) | [Italiano](../it/10-inference/infer-pi05.md) | 日本語 | [한국어](../ko/10-inference/infer-pi05.md) | [Português (BR)](../pt-br/10-inference/infer-pi05.md) | [Português (PT)](../pt-pt/10-inference/infer-pi05.md)

# 推論コマンドライン - pi0.5

## Ubuntu

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- 推論コマンドライン

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi05/50K/pretrained_model
```















## Mac

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論コマンドライン

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![この画像は、Ubuntu 環境で Python 3.12 と mujoco シミュレーション環境を使ってロボットを制御するコマンドラインインターフェースを示しています。コードの一部と、実行中に現れる警告やエラーメッセージ（addCriterion）が表示されています。](../../en/images/d52-01.png)
</column>
<column width-ratio="0.534183">
![この画像は、Ubuntu 環境で Python 3.7.12 と PyTorch 1.12.0 を使って関連コードを実行したときの出力を示しています。「WARNING」と「INFO」レベルの警告やメッセージがいくつか含まれています。](../../en/images/d52-02.png)
</column>
</grid>

## 推論が遅いのはなぜか

- データセットが小さすぎる
- GPU のメモリが足りない。50 番台のカードが必要です
