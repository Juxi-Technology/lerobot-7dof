[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | 日本語 | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Ubuntu コンピューター

## カメラをコンピューターに接続する

```Shell
lerobot-find-cameras opencv
```

![この画像は、Ubuntu のターミナルでのカメラ検出結果を示しています。コマンド「lerobot-find-cameras opencv」が実行され、Camera #0 という番号の OpenCV Camera という名前のカメラが検出され、パスは /dev/video0、種類は OpenCV、バックエンド API は V4L2 です。そのデフォルトのストリーム形式のパラメーターには、Fourcc 形式 YUYV、幅 640、高さ 480、フレームレート 30.0 が含まれます。最後に画像の保存が完了し、画像が outputs/captured_images ディレクトリに保存されたことが示されています。これは Ubuntu コンピューターで接続されているカメラを見つける内容に対応します。](../../en/images/d20-01.png)

## カメラ映像を表示しながらのテレオペレーション

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

## カメラ複数台で、カメラ映像を表示しながらのテレオペレーション

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
