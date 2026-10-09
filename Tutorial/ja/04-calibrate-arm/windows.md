[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | 日本語 | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Windows コンピューター



<callout emoji="🚫">
Leader アームと Follower アームの両方を接続する必要があります
</callout>

## Follower アームのキャリブレーション

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![この画像は、Windows コンピューターで lerobot のキャリブレーションを実行するコマンドラインインターフェースを示しています。コマンドは「lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm」です。インターフェースには「zihao_follower_arm SO10IFollower connected」などのプロンプトを含むキャリブレーション情報が表示され、ロボットアームの各関節の NAME、MIN、POS、MAX の値も一覧表示されています。キャリブレーション中は、ロボットアームを可動範囲の中間に移動して ENTER を押すよう促し、同時に位置を記録し、ENTER を押して停止します。この画像は Follower アームのキャリブレーションに関連し、具体的な手順とインターフェースのフィードバックを示しています。](../../en/images/d14-01.png)

## Leader アームのキャリブレーション

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![この画像は、Windows コンピューターで lerobot-calibrate コマンドを使ってロボットアームをキャリブレーションするコマンドラインインターフェースを示しています。Follower アームと Leader アームのキャリブレーションに関する情報として、キャリブレーション位置の保存パス、ロボットタイプ、ポート番号、ID が表示されています。また、Follower を可動範囲の中間に移動して ENTER を押し、各関節を可動範囲いっぱいに動かして位置を記録し、ENTER を押して停止するよう促しています。下部には各関節の名前、最小値、現在位置、最大値が表示されています。](../../en/images/d14-02.png)

## ファイルの出力先

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## 別のロボットアームのキャリブレーション

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## 補足

### ① アームが限界に達した後で動かなくなる

再キャリブレーションが必要です

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② サーボが見つからない

![この画像は、macOS で lerobot プログラムを実行したときのエラーメッセージを示しています。プログラムの実行中に RuntimeError「FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'」が発生し、欠けているサーボ ID（サーボ 1～6、いずれも期待される型番は 777）があること、しかし実際に見つかったサーボの一覧は空であることを示しています。これは「サーボが見つからない」という補足に関連し、サーボが接続されていないことが原因の可能性があるため、再接続してコネクターを回してみてください。](../../en/images/d14-03.png)

サーボの電源が接続されていません。再接続してコネクターを回してみてください
