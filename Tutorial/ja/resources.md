[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | 日本語 | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>各種リソース</title>

# Lerbot 公式 [URDF ファイル](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf)

## URDF Studio

https://urdf.d-robotics.cc/

## ROS2 シミュレーション制御（ご自身で実装）

https://github.com/holmsslk/so-arm-moveit-hardware

## LeRobot 公式グラフィカルインターフェース

https://github.com/huggingface/leLab

LeLab は、LeRobot のワークフロー全体 — キャリブレーション、テレオペレーション、記録、学習、再生 — を単一のブラウザインターフェースにまとめた Web アプリです。ロボットアームを接続してアプリを開くだけで、すぐに作業を始められます。面倒なコマンドライン作業もキーボード入力も不要です。

🤗 LeRobot のネイティブな Web エントリーポイントで、新規ユーザーが「箱から出してすぐ」の状態から「最初のポリシーを学習する」までを数分で行えるように設計されています。

🤗 コマンド1つですべてのインストールと実行が完了します。

# スマートフォンから Follower アームを制御する

https://huggingface.co/docs/lerobot/main/en/phone_teleop

## クラウドロボティクス開発：AWS 上の ROS 2 デバイスと Isaac Sim LeRobot シミュレーションおよびデータストリーミング

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md

## Web UI でサーボ ID と中心キャリブレーションを設定する

https://bambot.org/feetech.js?lang=zh

1. サーボのモデルに応じて 0 または 1 を入力し、「Connect」をクリックします

![この画像は、Web UI でサーボ ID と中心キャリブレーションを設定する際の接続インターフェースを示しています。インターフェースには「Connect」セクションがあり、ボーレートの選択ボックス（1,000,000 bps、Index 0）、プロトコル端の選択ボックス（0=STS/SMS、1=SCS、現在は 0 を選択）、接続ボタン（緑色、数字 2 のマーク付き）、および状態表示（赤色、「切断」と表示）が含まれています。この画像は、上記で述べた「サーボのモデルに応じて 0 または 1 を入力し、「Connect」をクリックする」と呼応し、接続操作のインターフェース設定状況を視覚的に示しています。](../en/images/d58-01.png)

2. ID 1\~6 のサーボをスキャンし、スキャン結果の FOUND で対応する ID のサーボを確認します。たとえば画像ではサーボ ID 1 が見つかっています

![これは Lerbot 関連ソフトのサーボスキャンインターフェースのスクリーンショットで、中心的な内容はサーボスキャンの操作過程と結果です。インターフェースには「開始 ID」「終了 ID」の入力欄があり、それぞれ 1 と 6 が入力され、「スキャン開始」ボタンもあります。その下が対応するスキャン結果の表示領域で、この位置は文書で言及されているサーボ ID がスキャンされた内容に対応します。スキャン結果のログによると、ID1 をスキャンしたときだけサーボ情報の取得に成功し、「FOUND: Pos-1230, Mode=0, BaudIdx=0」と表示され、ID2 から ID6 をスキャンしたときはすべて位置読み取り異常、ステータスパケットなしのエラーが表示されました。](../en/images/d58-02.png)

3. ID 設定と中心キャリブレーション

① 現在のサーボ ID の入力を、スキャンしたサーボの ID に設定します

② 「ID management」に数値を入力し、「Change ID」をクリックして ID を設定します

③ 中心キャリブレーション（STS3215 サーボの中心は 2047、SCS0009 サーボの中心は 511）

STS サーボ：「Position control」に 2047 を入力し、「Set」をクリックします

SCS サーボ：「Position control」に 511 を入力し、「Set」をクリックします

![この画像は Lerbot の単一サーボ制御インターフェースを示しています。「Current servo ID」は 1 と表示されています。その下の「ID management」には数値 1 と「Change ID」ボタンがあります。Position Control エリアには「Read position」ボタンがあり、位置 2047 が表示され、その隣に「Set」ボタンがあります。この画像は、文書の「ID 設定と中心キャリブレーション」のセクションに関連し、サーボ ID の設定と中心のキャリブレーションを行うためのインターフェースを視覚的に示し、ユーザーが Lerbot でこれらの設定を実行する方法を理解できるよう支援しています。](../en/images/d58-03.png)
