[English](../en/10-inference/infer-wall-oss.md) | [简体中文](../zh-hans/10-inference/infer-wall-oss.md) | [繁體中文](../zh-hant/10-inference/infer-wall-oss.md) | [Deutsch](../de/10-inference/infer-wall-oss.md) | [Español](../es/10-inference/infer-wall-oss.md) | [Français](../fr/10-inference/infer-wall-oss.md) | [Italiano](../it/10-inference/infer-wall-oss.md) | 日本語 | [한국어](../ko/10-inference/infer-wall-oss.md) | [Português (BR)](../pt-br/10-inference/infer-wall-oss.md) | [Português (PT)](../pt-pt/10-inference/infer-wall-oss.md)

# 推論コマンドライン - WALL-OSS

## Ubuntu

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論コマンドライン

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

![この画像は、Ubuntu 環境での推論コマンドラインセッション中のターミナルを示しています。動画の総ピクセル数が 90316800、前処理構成ファイルのサイズが 2.46 KB など、いくつかのパラメーター設定が表示されています。また、tokenizer.json や tokenizer_config.json などファイルの読み込み進捗も表示され、たとえば tokenizer.json は 100% まで読み込まれています。上部には「INFO」メッセージがあり、その下には「Loading model from:」などのモデル読み込みのプロンプトがあります。この画像は Ubuntu の推論コマンドラインに対応し、操作中の主要な情報を示しています。](../../en/images/d53-01.png)

<figure view-type="Preview">[Attachment: c7b8a795e52ca10689d296d212dc8e53.mp4](../../en/images/c7b8a795e52ca10689d296d212dc8e53.mp4)</figure>





## 推論コマンドライン - Mac

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論コマンドライン

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

<figure view-type="Preview">[Attachment: 9f4567abdcce7f983415aef8f42877d0.mp4](../../en/images/9f4567abdcce7f983415aef8f42877d0.mp4)</figure>
