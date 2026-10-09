[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | 日本語 | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# ローカル Ubuntu での学習

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- 注意

`\` の前には半角スペースを 1 つだけ入れ、後ろにはスペースを入れないでください

`--dataset.split` のデフォルトは `train` で、データセット全体を学習セットとして使用することを意味します

データセットはローカルにあるため、`--dataset.streaming` は `false` にする必要があります。データセットはすでにディスク上にあり、ストリーミング読み込みは不要だからです

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![この画像は、ローカルの Ubuntu 環境で lerobot_train.py スクリプトを実行するための学習コマンドと構成を示しています。コマンドにはデータセットのパス、リポジトリ ID、ブランチなどのパラメーターが含まれており、たとえば `--dataset.repo_id` は Tommy/lerobot_zhao_dataset_a に設定されています。構成値の中では、`--dataset.streaming` が `false`、`--use_imagenet_stats` が `True`、`--batch_size` が 4、`--val_n_episodes` が 1000 に設定されています。この画像は文脈と密接に関連し、学習コマンドとその主要な構成パラメーターを視覚的に示しています。](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![この画像は、ローカルの Ubuntu 環境で lerobot_train.py スクリプトを実行して生成された学習ログを示しており、学習の構成パラメーターと学習実行のライブ状態を中心としています。主要な学習情報が明確に示されています。`--dataset.split` のデフォルトは `train` で、データセット全体を学習セットとして使用することを意味し、データセットがローカルに保存されているため `--dataset.streaming` の状態も同様に固定されます。ログには学習の進捗、データセットの読み込み、モデルのオプティマイザーとスケジューラーの作成、学習中の loss と step の数値も含まれており、進行中のローカル学習の様子がはっきりと分かります。](../../en/images/d34-02.png)
</column>
</grid>

![この画像は、Ubuntu 環境で LeRobot を使ってモデルを学習するときに生成された学習ログを示しています。ログには、時刻、学習セット、モデル、loss、精度など、学習実行の情報が記録されています。タイムスタンプは 2024 年 1 月 14 日の 15:11:53 から 16:15:16 までで、loss は 0.68 から 0.65 の間で変動し、精度（acc）は 0.85 から 0.88 の間で変動しています。この画像は LeRobot モデルの学習の文脈に関連し、学習中に主要な指標がどのように変化するかを視覚的に示しています。](../../en/images/d34-03.png)
