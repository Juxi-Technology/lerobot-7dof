[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | 日本語 | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Mac コンピューター

## ポートの確認

```Shell
ls /dev/tty.*
```

結果は下の画像のようになります。どちらのポートでも使用できます

![この画像は、Mac のターミナルで「ls /dev/tty.*」コマンドを実行した後の出力で、いくつかのポートが一覧表示されています。そのうち「/dev/tty.usbmodem5AAF2194741」と「/dev/tty.wchusbserial5AAF2194741」の 2 つが赤枠で強調されています。ポートの確認という文脈で、結果はこの画像と同様であり、2 つのポートのどちらでも使用できます。この画像は確認すべきポート情報を視覚的に示しており、上記の「ポートの確認」の内容と密接に関連し、ポート確認操作の結果を示したものです。](../../en/images/d09-01.png)

## ポートへの権限の付与

これらのシリアルデバイスに対して、すべてのユーザーに読み書きの権限を与えます

```Shell
chmod 666 /dev/tty.*
```

## 自分のポートを記録する

Follower アーム：

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader アーム：

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Mac でポートが 2 つ表示されるのはなぜか？

私たちが使用しているサーボコントローラーボードは、**macOS から同時に 2 種類の異なるシリアルドライバーとして認識される**ため、2 つのポートが表示されます：

- 1 つはシステム標準の汎用シリアルドライバー（`/dev/tty.usbmodemxxxx`）です
- もう 1 つはチップベンダーが提供する専用のシリアルドライバーです（たとえば、ここでの「wch」は南京沁恒の CH340/CH341 チップを指します）（`/dev/tty.wchusbserialxxxx`）

これは正常な動作です。**2 つのポートは実際には同じハードウェアデバイスに対応しており**、どちらからでも接続して通信できます（たとえば、ロボットアームを制御するソフトウェアではどちらかのポートを選ぶだけでかまいません）。

後で一方のポートを使っていてエラーが発生した場合は、もう一方に切り替えてみてください。
