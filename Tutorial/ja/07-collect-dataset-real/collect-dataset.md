[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | 日本語 | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# デモによるデータセットの収集

## 同名の既存データセットがある場合は削除する

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## カメラ 1 台でデータセットを収集する - Mac コンピューター

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## カメラ 2 台でデータセットを収集する - Mac コンピューター

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=true \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## 収集中

<grid>
<column width-ratio="0.508765">
![この画像は、Mac で OpenVSLAM を使ってデータセットを収集しているときのターミナルインターフェースを示しています。上部には解像度、フレームレート、エンコーダーなどの収集パラメーターが表示されています。以下は収集ログで、収集開始時刻、バージョン情報、スレッド数、エンコーダーが記録され、収集の進捗、たとえば 298/298 エピソードを収集し、合計 5119.33 秒かかったことも示されています。下部には「ESC」キーに関する注記があり、即座に停止してデータセットをアップロードするなどの説明があります。この画像はデータセット収集のワークフローに関連し、収集中のターミナルのフィードバックを視覚的に示しています。](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![この画像は Mac のコマンドラインターミナルインターフェースで、カメラのデータセット収集に関連する実行ログ情報を表示するために使われています。SVT 関連の設定パラメーターとして、設定パラメーター、エンコードライブラリのバージョン、各設定項目の値（キーフレームや CRF、エンコード解像度など）が含まれ、実行時ステータスのログ、たとえば MP4 ファイルの処理に関するメッセージや、プログラム実行中のデバイス切断の記録とタイムスタンプも示されています。全体として、カメラのデータセット収集中のバックグラウンドの実行状態を示しています。](../../en/images/d26-02.png)
</column>
</grid>

キーボードの矢印キー操作：  
→（右矢印）現在のエピソードを途中で終了し、次のエピソードへ進みます。  
←（左矢印）現在のエピソードをキャンセルし、録り直します。  
ESC、即座に停止し、動画をエンコードしてデータセットをアップロードします。

## 収集完了 — データセットの保存ディレクトリ

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```





## Handshake

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.num_episodes=30 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```
