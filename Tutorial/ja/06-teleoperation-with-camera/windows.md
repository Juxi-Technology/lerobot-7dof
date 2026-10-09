[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | 日本語 | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Windows コンピューター

## カメラをコンピューターに接続する

```Shell
lerobot-find-cameras opencv
```

![この画像は Windows のコマンドラインのエラー画面で、文書で言及されている「カメラが接続できない」というエラーシナリオに対応しています。画面に表示されたエラー情報は 2 つの部分からなります。1 つは全体エラー「Camera index out of range」、もう 1 つは lerobot 関連スクリプトのエラー「Failed to connect or configure OpenCV camera 0」です。画面には検出されたカメラ情報も一覧表示されており、OpenCV Camera という名前のデバイスが 2 つあり、どちらも DSHOW バックエンド、形式は YUY2、幅 640、高さ 480、フレームレート 0.0FPS です。このエラー内容は、文書で説明されているカメラが接続できないのに Tencent Meeting では正常に使える状況に対応しています。](../../en/images/d22-01.png)

## カメラ映像を表示しながらのテレオペレーション

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

rerun.io のウィンドウが開き、各サーボ関節の軌道がリアルタイムで表示され、カメラのライブ映像も表示されます

さらに画像は `C:\Users\username\outputs\captured_images` ディレクトリに保存されます

![この画像は rerun.io の画面で、各サーボ関節の軌道とカメラのライブ映像がリアルタイムに表示されています。画面の左側はブループリントのインターフェースで、「teleoperation」などのオプションがあります。中央は軌道のグラフで、「observation_wrist_rot.pos」などの関節位置データが表示されています。右側はカメラ映像で、ロボット視点のシーンが映っています。画面の右上には「Waiting for data on rerun: http://127.0.0.1:9876/remote...」と表示され、その下にデータソース情報があります。この画像は、rerun.io の画面がカメラ映像をリアルタイムで表示する内容の説明に関連し、その画面効果を視覚的に示しています。](../../en/images/d22-02.png)

## 以下のエラーが発生した場合

カメラが接続できませんが、Tencent Meeting でカメラを切り替えると、それでも正常に開けます

![この画像は Windows コンピューターのコマンドラインインターフェースで、カメラ検出の結果を示しています。画面上部には「管理者：コマンドプロンプト - C:\Users\40743\miniconda3\condabin\conda.bat activate leroobot」という文字があります。下部には「Detected Cameras」情報が出力され、「Camera #0」は OpenCV Camera @ 0、Type は OpenCV、Backend api は DSHOW、Default stream profile などのパラメーターが一覧表示され、さらに「ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)」などのエラーメッセージも表示されています。この画像は、文書でカメラが接続できない問題に遭遇した文脈に関連し、関連するエラー情報を視覚的に示しています。](../../en/images/d22-03.png)

`lerobot\src\lerobot\cameras\utils.py` ファイルを修正し、OpenCV バックエンドを `cv2.CAP_SHOW` に変更します

![この画像は、lerobot ライブラリ内の `utils.py` ファイルを修正するための Python コードの一部を示しています。この内容は、文書で Windows コンピューターのカメラ接続エラーに対処する手順に対応しています。コードは `get_cv2_backend()` 関数を定義し、システムごとに OpenCV バックエンドを設定します。システムが Windows の場合、もともとコメントされていた `return int(cv2.CAP_MSMF)` はコメントアウトされ、現在ハイライトされている内容は `return int(cv2.CAP_DSHOW)` で、その横にはこの方式で AVFOUNDATION を置き換えることを説明する注記があります。この関数は macOS と Linux システムにも対応する OpenCV バックエンド方式を設定しており、指定したカメラの接続問題を解決するための核心となるコード修正内容です。](../../en/images/d22-04.png)

> これは Doubao でも解決できないバグです。ひとえに lerobot ライブラリのラップが深すぎるためで、初心者にとってデバッグは非常に困難です

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## カメラ複数台を接続して、カメラ映像を表示しながらのテレオペレーション

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
