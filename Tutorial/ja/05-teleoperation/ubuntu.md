[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | 日本語 | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Ubuntu コンピューター

## ポートへの権限の付与

これらのシリアルデバイスに対して、すべてのユーザーに読み書きの権限を与えます

```Shell
sudo chmod 666 /dev/ttyACM*
```

## テレオペレーション

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![この画像は、Ubuntu コンピューターでポートに権限を与え、テレオペレーションを行うときのインターフェースを示しています。まずコマンド `sudo chmod 666 /dev/ttyACM*` を実行してシリアルデバイスに権限を与えます。次に `lerobot-teleoperate` コマンドが入力され、ロボットの id「zihao follower arm」とポート「/dev/ttyACM0」、teleop の id「zihao leader arm」とポート「/dev/ttyACM1」など、ロボットと teleop の情報が表示されています。この画像はポートへの権限付与とテレオペレーションの内容と密接に関連し、操作とその結果を視覚的に示しています。](../../en/images/d16-01.png)
