[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | [Deutsch](../de/10-inference/infer-smolvla.md) | [Español](../es/10-inference/infer-smolvla.md) | [Français](../fr/10-inference/infer-smolvla.md) | [Italiano](../it/10-inference/infer-smolvla.md) | 日本語 | [한국어](../ko/10-inference/infer-smolvla.md) | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# 推論コマンドライン - smolvla

## Ubuntu

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## Mac

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論コマンドライン

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/smolvla/40K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=2000
```

<grid>
<column width-ratio="0.425772">
![この画像は、Ubuntu 環境での推論コマンドラインインターフェースを示しています。上部には実行中のコマンドが表示され、キャッシュの使用や Delta Joint Actions Aloha の使用などのパラメーター設定が含まれています。その下にはいくつかの重要な情報があり、ロボット id の「zihao_follower_arm」、最大相対ターゲットが None、ポート「/dev/tty.usbmodemSAAF2193661」、VLM レイヤー数が 16 に削減されたという注記などが示されています。この画像は Ubuntu の推論コマンドラインに関連し、インターフェースと一部の主要なパラメーター設定を示しています。](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![この画像は、Ubuntu 環境での推論コマンドラインセッション中のターミナルを示しています。上部には calibration_dir や cameras などロボット関連の構成が表示されています。その下には config.json や processor_config.json などいくつかの json ファイルの読み込みプログレスバーがあり、読み込みの割合とサイズが示されています。最下部には「Mismatch between calibration values in the motor and the calibration file or no calibration file found」といったログメッセージがあり、モーターのキャリブレーション値とキャリブレーションファイルの不一致を知らせています。この画像は Ubuntu の推論コマンドラインに対応し、操作中のターミナルのフィードバックを示しています。](../../en/images/d50-02.png)
</column>
</grid>

## 結果

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
