[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | 日本語 | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# データセットの表示と再生

## データセット全体の可視化

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

`TommyZihao/lerobot_zihao_dataset_a`、または別のデータセットを入力します

![この画像は LeRobot Dataset Visualizer のインターフェースを示しており、画像にはロボットが写り、上部に「LeRobot Dataset Visualizer」の文字があります。中央にはデータセットのオプションを示すドロップダウンメニューがあり、「TommyZihao/lerobot_zihao_dataset_a」などのほか、その下に「Example Datasets」とデータセット名があります。その下には青い「Explore Open Datasets」ボタンがあります。この画像は上記のデータセット全体の可視化に関連し、指定したデータセットを入力する操作に対応します。](../../en/images/d29-01.png)

![addCriterion addCriterion を示した画像](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![この画像は LeRobot のオレンジをつかむデータセットの可視化インターフェースを示しています。上部にはオレンジをつかむ動画があり、オレンジが白い物体に寄せられています。以下にはデータグラフがあり、「actuator」、「gripper」、「gripper_pos」などいくつかの変数の時間変化の曲線が表示されています。左側には指示の一覧があり、現在は「Grab Orangesanges」が選択されています。再生と一時停止のボタンは右下にあります。この画像は特定のエピソードの可視化に関連し、つかむ動作とそれに対応するデータを視覚的に示しています。](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![この画像は LeRobot データセットの可視化インターフェースを示しています。左側には、異なる時刻の画面を表示するためにドラッグできるタイムラインがあり、中央には動いている 2 本の手を映したカメラ映像があります。](../../en/images/d29-04.png)
</column>
</grid>

注意：command と state は同じではありません。command は Leader アームによって提供され、state は Follower アームによって提供されます

## 特定のエピソードの可視化

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![この画像は rerun.io プラットフォームで特定のエピソードを可視化するインターフェースを示しています。左側はデータセットの構造で、observation_images などのデータが表示されています。中央上部はライブカメラ映像で、画像にはオレンジが写っています。右側はデータの曲線で、異なるデータが時間とともにどのように変化するかを示しています。下部はタイムラインで、任意の時点のデータを表示するためにドラッグできます。この画像は「特定のエピソードの可視化」に対応し、特定のエピソードを表示するときのインターフェースとデータを視覚的に示しています。](../../en/images/d29-05.png)

タイムラインをドラッグして、任意の時点のカメラ映像とサーボ位置を表示します

## 特定のエピソードで Follower アームの動作を再生する

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

`Replaying episode` と表示された後、Follower アームが動き、指定したエピソードの動作を再生・再現します

実際、この時点で多くの素人を驚かせることができるのではないでしょうか

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
