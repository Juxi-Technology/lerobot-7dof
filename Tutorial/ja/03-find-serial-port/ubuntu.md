[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | 日本語 | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# 方法 1：Linux コマンドラインから直接確認する

## シリアルデバイスポートの確認

```Shell
ls /dev/ttyACM*
```

## コンピューターとロボットアームの USB ポートを接続する

まず Follower アームを接続し、次に Leader アームを接続します

![この画像は、Ubuntu で Linux コマンドラインを使ってシリアルデバイスポートを確認する様子を示しています。まず「何も接続していない」状態が示され、次に Follower アームを接続すると「/dev/ttyACM0」が現れます。その後 Leader アームを接続すると「/dev/ttyACM1」が現れます。この画像は文脈と密接に関連し、コンピューターとロボットアームの USB ポートを接続した後に、コマンドラインからシリアルデバイスポートがどのように見えるかを視覚的に示し、Linux コマンドラインからシリアルデバイスポートを確認する方法の理解を助けます。](../../en/images/d08-01.png)

# 方法 2：公式 LeRobot ツール

```Shell
lerobot-find-port
```

![この画像は、Ubuntu で公式 LeRobot ツールを使ってシリアルデバイスポートを確認するコマンドラインインターフェースを示しています。コマンドは「lerobot-find-port」で、複数の「/dev/ttyACM」ポートを含む利用可能なすべてのポートが表示されています。プロンプトは、Follower の USB ケーブルを抜いてそのポート番号「/dev/ttyACM0」を確認し、その後 USB ケーブルを再び接続するよう促しています。この画像はシリアルデバイスポートを確認する方法 2 に関連し、その手順と結果を視覚的に示しています。](../../en/images/d08-02.png)

![この画像は、Ubuntu で `lerobot-find-port` コマンドを使ってロボットアームのシリアルデバイスポートを確認する様子を示しています。コマンドを実行すると、利用可能なすべてのポートが一覧表示され、次に Leader の USB ケーブルを抜くよう促され、最後に Leader アームのシリアルデバイスポート番号として `/dev/ttyACM1` が表示されています。この画像はシリアルデバイスポートを確認する方法 2 に関連し、公式 LeRobot ツールでポート番号を取得した結果を視覚的に示しています。](../../en/images/d08-03.png)

# 自分のポートを記録する

`/dev/ttyACM0` は Follower アームのシリアルデバイスポート番号です

`/dev/ttyACM1` は Leader アームのシリアルデバイスポート番号です

# ポートへの権限の付与

これらのシリアルデバイスに対して、すべてのユーザーに読み書きの権限を与えます

```Shell
sudo chmod 666 /dev/ttyACM*
```
