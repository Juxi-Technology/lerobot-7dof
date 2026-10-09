[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | 日本語 | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Mac コンピューター

## ポート番号の確認

Follower アーム：

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader アーム：

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Follower アームのキャリブレーション

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![この画像は、Mac で SO101 サーボをキャリブレーションするコマンドラインインターフェースを示しています。コマンドは「lerobot-calibrate」で、パラメーターには robot.type、robot.port、robot.id が含まれます。インターフェースには「zihao_follower_arm」などのロボット設定情報が表示されています。下部では「c」と Enter を押してキャリブレーションを開始するよう促しており、「zihao_follower_arm SO101Follower connected」などのメッセージも表示されています。この画像は「Follower アームのキャリブレーション」セクションに対応し、キャリブレーションコマンドとインターフェースのフィードバックを視覚的に示しています。](../../en/images/d13-01.png)

![この画像は、Ubuntu での LeRobot キャリブレーション操作のコマンドラインインターフェースを示しています。コマンドは「lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm」で、各関節の最小値、最大値、現在位置を含む Follower のキャリブレーション情報が表示されています。重要な操作のプロンプトは赤枠で強調されており、たとえば「Enter を押してキャリブレーションを開始」「各関節を順に上限と下限まで動かす」「Enter を押してキャリブレーションを終了」などがあり、文脈で説明されているキャリブレーション手順と対応しています。](../../en/images/d13-02.png)

## Leader アームのキャリブレーション

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![この画像は、Ubuntu での LeRobot キャリブレーションのコマンドラインインターフェースを示しています。コマンドラインでは「sudo chmod 666 /dev/ttyACM*」や「lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1」などの操作が実行され、Follower アームと Leader アームのポート番号情報が表示されています。インターフェースでは、Enter を押してキャリブレーションを開始し、各関節を順に上限と下限まで動かし、Enter を押して終了するよう促しており、最後にキャリブレーション設定ファイルが保存されるパスが表示されています。この画像は LeRobot のキャリブレーション内容に関連し、キャリブレーション手順を視覚的に示しています。](../../en/images/d13-03.png)

## キャリブレーション設定ファイルの確認

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## よくある不具合

- 1 つまたは複数のサーボが見つからない

![この画像は、SO Follower のキャリブレーション中に表示されるサーボパラメーター情報を示しています。上部には接続情報と、Follower を可動範囲の中間に移動して ENTER を押し、その後すべての関節の可動範囲を順に通して位置を記録し、ENTER を押して停止するよう促すキャリブレーションのプロンプトが表示されています。下の表には、shoulder_pan、shoulder_lift、elbow_flex、wrist_flex、gripper などのサーボの NAME、MIN、POS、MAX の値が一覧表示されています。この画像は Follower アームのキャリブレーションに関連し、キャリブレーション中のパラメーターを視覚的に示しています。](../../en/images/d13-04.png)



## 補足

### ① アームが限界に達した後で動かなくなる

再キャリブレーションが必要です

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② サーボが見つからない

![この画像は、LeRobot のロボットコードを実行したときのエラーメッセージが表示された Mac のターミナルインターフェースを示しています。エラーは、ポート '/dev/tty.usbmodem5AAF2193061' で FeetechMotorsBus のモーター確認が失敗し、モーター ID -1 から -6 が欠けており、期待されるモデルが 777 であることを示しています。また、期待されるモーターの完全な一覧と、実際に見つかったモーターの完全な一覧も表示されています。この画像は「よくある不具合」セクションに関連し、「サーボが見つからない」問題が実行時エラーとしてどのように現れるかを視覚的に示しています。](../../en/images/d13-05.png)

サーボの電源が接続されていません
