[English](../en/09-train-model/wandb-training-curves.md) | [简体中文](../zh-hans/09-train-model/wandb-training-curves.md) | [繁體中文](../zh-hant/09-train-model/wandb-training-curves.md) | [Deutsch](../de/09-train-model/wandb-training-curves.md) | [Español](../es/09-train-model/wandb-training-curves.md) | [Français](../fr/09-train-model/wandb-training-curves.md) | [Italiano](../it/09-train-model/wandb-training-curves.md) | 日本語 | [한국어](../ko/09-train-model/wandb-training-curves.md) | [Português (BR)](../pt-br/09-train-model/wandb-training-curves.md) | [Português (PT)](../pt-pt/09-train-model/wandb-training-curves.md)

# wandb で学習曲線をリアルタイム表示する

- wandb のリンクを取得する

![この画像は、ブラウザーで開いた LeRobot の学習インターフェースを示しています。「Lerobot - dataset repo_id = x」の実行ログが表示され、その中で「one will be synced with wandb」や「Creating dataset」といった主要な情報が赤い枠で強調されています。この画像は「学習曲線をリアルタイム表示する」セクションに関連し、LeRobot の学習中にブラウザーで学習曲線をリアルタイムに確認できることを説明しています。この画面は、その曲線を表示するときに見られる実行ログのインターフェースです。](../../en/images/d44-01.png)

- 学習曲線をリアルタイムで表示する

![この画像は、wandb で学習曲線をリアルタイム表示するためのインターフェースを示しています。上部には「Charts」「Overview」「Logs」「Files」などのタブがあり、現在は「Charts」が選択されています。その下には「train/update_x」「train/loss」「train/samples」「train/ir」「train/loss」「train/TL_loss」など複数のチャートがあり、それぞれ学習中の関連データの変化を折れ線グラフで示しており、横軸が Step、縦軸が各種指標です。この画像は、上記の「wandb のリンクを取得する」と「学習曲線をリアルタイムで表示する」の内容に関連し、学習曲線を視覚的に示しています。](../../en/images/d44-02.png)
