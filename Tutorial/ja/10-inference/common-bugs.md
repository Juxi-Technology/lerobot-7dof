[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | 日本語 | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# よくある不具合と対処法

## カメラのキャプチャーに失敗する

![この画像は、LeRobot のロボット関連コードを実行したときのターミナル出力を示しています。そのうち `INFO` ログでは OpenCV カメラが開き、Follower が切断されたなどの情報が示されています。`ERROR` ログでは、`camera_opencv.py` ファイル内の `read` 関数が `OpenCVCamera(0) read failed` のために `RuntimeError` を発生させたことが指摘されています。この画像は、文書の「カメラのキャプチャーに失敗する」問題に関連し、コード実行時に現れるエラー状況を視覚的に示し、この問題が実際の動作でどのように現れるかの理解を助けます。](../../en/images/d55-01.png)

手首カメラのケーブルが緩んでいないか確認してください。特にカメラ側の端は接触不良を起こしやすい箇所です

## カメラが切断される

![この画像は、LeRobot HF の実行中に発生したカメラ取得失敗問題のコードとエラー情報の画面を示しています。画面上部には OpenCV カメラの接続失敗の表示があり、下部は `lerobot_record.py` ファイル内の `record_loop` 関数のエラースタックで、`async_read` 時に `TimeoutError` が発生し、カメラが 200ms 以内にフレームを取得できなかったことが示されています。この画像は、文書の「カメラのキャプチャーに失敗する」問題の文脈に対応し、この問題がコードレベルでどのように現れるかを視覚的に示しています。](../../en/images/d55-02.png)

コマンドラインを再起動します

## サーボ通信の問題 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![この画像はターミナルのエラーログの一部で、文書で言及されているサーボ通信の問題 1 の内容に対応しています。ログには具体的なコードファイルのパスが表示され、`lerobot/src/lerobot/motors/motors_bus.py` ファイルの 1069 行目、1181 行目に `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries` というエラー情報が記録され、さらに `There is no status packet!` というエラー原因も示されており、文書で言及されたサーボ通信の問題 1 のエラー詳細を完全に示しています。](../../en/images/fix-04.png)

対処法：`lerobot/src/lerobot/motors/motors_bus.py` のコード内にあるすべての `num_retry` を 99 に変更します。特にエラーが出ている行のものを変更してください

![この画像は、LeRobot プロジェクト内の `motors_bus.py` コードファイルの内容を示しています。ファイル内の `MotorsBusABC` クラスの `write` メソッドがハイライト表示され、その中の `num_retry` 変数が `99` に変更されています。この画像は、文書の「サーボ通信の問題 1」の部分に関連し、解決策で言及されている `lerobot/src/lerobot/motors/motors_bus.py` のコード内のすべての `num_retry` を 99 に変更する操作、特にエラー行に対応するコード部分に対応しています。](../../en/images/d55-03.png)

## サーボ通信の問題 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![この画像はエラーログの一部で、文書の「サーボ通信の問題 2」に対応するエラー内容に属します。具体的には、コード実行時に ConnectionError というエラーが発生し、Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries という形で現れ、付随するエラー情報には There is no status packet と書かれ、さらにこのエラーを引き起こしたコードの場所も示されています。このエラーログは、文書の「サーボ通信の問題 2」の説明に完全に対応し、この問題の実際のエラー記録です。](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![この画像は、macOS 環境で zsh ターミナルを使ってコマンドライン操作を実行した画面を示しています。ターミナルには複数のファイルパスとコードファイルの内容が表示され、うち `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` ファイルに `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!` というエラー情報があります。この画像は、文書の「サーボ通信の問題 2」の内容に関連し、エラー発生時のターミナル画面の状況を視覚的に示しています。](../../en/images/fix-05.png)
</column>
</grid>

解決策：ロボットアームを再キャリブレーションします
