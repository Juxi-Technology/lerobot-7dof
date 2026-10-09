[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | 日本語 | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Ubuntu コンピューター

## ポートへの権限の付与

これらのシリアルデバイスに対して、すべてのユーザーに読み書きの権限を与えます

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Follower アームのキャリブレーション

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![この画像は、Ubuntu コンピューターで「lerobot-calibrate」コマンドを実行して Follower アームをキャリブレーションするターミナルインターフェースを示しています。Follower の接続情報、関節名、上限と下限の値が表示されています。重要な情報には、Enter を押してキャリブレーションを開始、各関節を順に上限と下限まで動かす、Enter を押してキャリブレーションを終了、といったものや、「Calibration saved to」などのキャリブレーションファイルのパス情報が含まれます。この画像は Follower アームのキャリブレーション手順と密接に関連し、キャリブレーション中のターミナルのフィードバックを視覚的に示しています。](../../en/images/d12-01.png)

## Leader アームのキャリブレーション

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![この画像は、Ubuntu コンピューターでポートに権限を与えた後のインターフェースを示しています。コマンドラインで「sudo chmod 666 /dev/ttyACM*」が入力され、実行後に「zihao_leader_arm」などの情報が表示されています。以下には「Enter を押してキャリブレーションを開始」「各関節を順に上限と下限まで動かす」「Enter を押してキャリブレーションを終了」などのプロンプトと、「Calibration saved to」などのキャリブレーション関連のパス情報が表示されています。この画像は「Leader アームのキャリブレーション」セクションに対応し、キャリブレーション前の準備インターフェースを視覚的に示しています。](../../en/images/d12-02.png)

## キャリブレーション設定ファイルの確認

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![この画像は、Ubuntu のターミナルに表示された「zihao_follower_arm.json」ファイルの内容を示しています。ファイルには shoulder_pan、shoulder_lift、elbow_flex、wrist_flex など複数のアームの設定情報が含まれ、各アームには id、drive_mode、homing_offset、range_min、range_max などのパラメーターがあります。この画像は「キャリブレーション設定ファイルの確認」セクションに関連し、キャリブレーションファイル内の具体的なパラメーター情報を視覚的に示し、各アームの設定の理解を助けます。](../../en/images/d12-03.png)



## 補足

### ① アームが限界に達した後で動かなくなる

再キャリブレーションが必要です

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② サーボが見つからない

![これは、Ubuntu のターミナルでエラーインターフェースを示すスクリーンショットで、「サーボが見つからない」という補足に対応しています。インターフェースには RuntimeError、具体的には「FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'」、すなわちサーボの確認が失敗したことが報告されています。また、期待されるサーボ情報として、期待されるモーター ID が 1～6、期待されるモデルが 777 であることが示されていますが、実際に見つかったモーターの一覧は空です。文脈と合わせると、このエラーはサーボに通電されていないことが原因です。](../../en/images/d12-04.png)

サーボの電源が接続されていません
