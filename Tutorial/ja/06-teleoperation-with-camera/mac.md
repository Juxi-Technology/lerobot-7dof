[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | 日本語 | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Mac コンピューター

## カメラをコンピューターに接続する

```Shell
lerobot-find-cameras opencv
```

![この画像は、Mac にカメラを接続した後の検出結果を示しています。画面には 2 つのカメラ情報が一覧表示されており、それぞれ外部接続のカメラと Mac 内蔵のフロントカメラです。外部カメラの Fps は 60.00024、内蔵カメラの Fps は 30.0 です。この画像は、文書で Mac にカメラを接続する内容に関連し、接続後のカメラ検出状況を視覚的に示し、カメラの種類、ID、バックエンド API などの情報や各カメラのフレームレートの理解を助けます。](../../en/images/d21-01.png)

## カメラ 1 台で、カメラ映像を表示しながらのテレオペレーション

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

実行すると、テレオペレーションが開始されます

rerun.io のウィンドウが開き、各サーボ関節の軌道がリアルタイムで表示され、カメラのライブ映像も表示されます

さらに画像は `~/username/outputs/captured_images` ディレクトリに保存されます

![この rerun.io の画面は、文書のテレオペレーションのカメラ機能に関する内容に対応し、カメラ 1 台でのテレオペレーション時のリアルタイム表示インターフェースです。画面の左側は折れ線グラフの領域で、各サーボ関節の軌道変化の曲線が表示され、色分けされた凡例が各種の関節データを区別しています。右側は上下 2 つのウィンドウに分かれ、それぞれカメラが撮影した屋内のリアルタイム映像を表示しています。下部はタイムラインとストリーム情報の領域で、同時にこのシーンはカメラが取得した画像を指定の `~/username/outputs/captured_images` ディレクトリに保存することに対応しています。](../../en/images/d21-02.png)

## カメラ複数台で、カメラ映像を表示しながらのテレオペレーション

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
