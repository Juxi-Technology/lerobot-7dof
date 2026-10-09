[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | 日本語 | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# デモによるデータセットの収集 — Handshake 200

## HuggingFace にデータセットリポジトリを作成する

https://huggingface.co/new-dataset

![この画像は、HuggingFace で新しいデータセットリポジトリを作成するインターフェースを示しています。「Owner」は TommyZihao、データセット名は「lerobot_zihao_dataset_shake200」、ライセンスは mit に設定され、「Public」オプションが選択されており、誰でもこのデータセットを閲覧できる一方、コミットできるのはデータセットの所有者または組織のメンバーだけです。以下には、データセットを作成した後にウェブインターフェースまたは git でファイルをアップロードできることと、下部に「Create dataset」ボタンがあることが記載されています。この画像は HuggingFace でデータセットリポジトリを作成する内容に関連し、データセット作成操作のインターフェースを示しています。](../../en/images/d27-01.png)

## 同名の既存データセットがある場合は削除する

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Shake200 データセットの収集

カメラ 1 台でデータセットを収集する - Mac コンピューター

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
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake200 \
    --dataset.num_episodes=200 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```

## 収集中

<grid>
<column width-ratio="0.508765">
![これは、握手データセット（Shake200）収集時のターミナル画面収録の内容です。画面の上部と中央やや下に「INFO 202-04-15 17:00:22.227 Recording episode 36」と表示され、36 番目のデータ収集サイクル（episode）を録画中であることが示されています。同時に SVT-AV1 エンコーダーの関連パラメーターと、進捗情報「200/200 \[00:00:00::00, 4911.09 examples/s\]」も表示されており、この段階がデータセット収集中であることを示し、文書に記述された「収集中」の工程に対応し、収集時のシステム動作状態を表しています。](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![この画像は Mac のターミナルの動作画面で、SVT プロジェクトの設定と実行ログ情報を示しています。画面には、SVT 設定の GOP サイズ、mini-pod サイズ、キーフレームタイプ、CRF などの数値や、SVT-A1 Encoder ライブラリのバージョン情報など、複数の設定パラメーターが含まれています。画面下半分のログには動画ファイル操作に関する内容が記録され、moov アトムをファイルの先頭へ移動することや、複数の OpenCVCamera、SV280 follower などのコンポーネントの接続・切断状態が言及されています。最後に現在のパスのターミナル実行終了の表示があり、デモによる握手 200 データセット収集時のターミナル動作シーンに対応しています。](../../en/images/d27-02.png)
</column>
</grid>

キーボードの矢印キー操作：  
→（右矢印）現在のエピソードを途中で終了し、次のエピソードへ進みます。  
←（左矢印）現在のエピソードをキャンセルし、録り直します。  
ESC、即座に停止し、動画をエンコードしてデータセットをアップロードします。

## 収集完了 — データセットの保存ディレクトリ

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
