[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | 日本語 | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# 学習コマンドライン - ACT（初心者に推奨）

## 参考ドキュメント

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## なぜ ACT アルゴリズムから始めるのか

ACT は、LeRobot を始めたときに最初に学習させるモデルとして最も推奨されるものです。その利点は次のとおりです。

- モデルが非常に軽量で、学習可能なパラメーターは 8000 万個しかありません
- 学習が速く収束し、推論も高速です
- 単一の GPU で 1 時間学習させるだけで結果を確認できます
- ACT モデルのアーカイブは約 200 MB で、保存や転送が容易です
- データは 30 エピソード程度収集すれば十分なことがほとんどです
- Ubuntu ホスト、Mac、Windows PC、さらには Raspberry Pi でも推論用にデプロイできます
- 実機での推論はかなり良好に動作し、把持・握手・ペン置きといった単純なタスクには十分すぎるほどです
- ACT アルゴリズムは LeRobot の基本環境にすでに組み込まれているため、追加のライブラリは不要です

## コマンドライン

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## コマンドラインの注意点

行継続の `\` の前には半角スペースを 1 つだけ入れ、その後ろにはスペースを入れないでください

赤字で示されたパラメーターは、実行のたびに確認または変更する必要があります

| コマンドラインパラメーター | 説明 |
|-|-|
| --dataset.repo_id | HuggingFace データセットの repo_id |
| --dataset.root | データセットのローカルパス |
| --dataset.revision | データセットのバージョン。HuggingFace にデータセットをアップロードしたときに指定したものです |
| --dataset.streaming | データセットはローカルにあるため、`false` にする必要があります。データはすでにディスク上にあり、ストリーミング読み込みは不要だからです |
| --dataset.split | デフォルトは `train` で、データセット全体を学習セットとして使用することを意味します |
| --policy.type | 学習するアルゴリズム（act、smolvla、diffusion、pi0、wallx など） |
| --output_dir | 出力構造を保存するディレクトリ |
| --job_name | この学習ジョブの名前 |
| --policy.device | 計算デバイス |
| --wandb.enable | wandb の可視化を有効にする |
| --wandb.project | wandb プロジェクト名 |
| --policy.push_to_hub | 学習済みモデルを HuggingFace にプッシュする |
| --steps | 学習ステップ数 |
| --batch_size | 1 ステップあたりに投入するデータ量。GPU メモリが不足する場合は小さくしてください |
|  |  |

## 学習プロセス

<grid>
<column width-ratio="0.357753">
![この画像は、コマンドラインから `lerobot-train` 学習コマンドを使用する例を示しています。このコマンドでは、`--dataset.repo_id` や `--dataset.root` などいくつかのパラメーターを設定してデータセットの詳細を指定し、`--policy.type` を `act` に、`--output_dir` を出力ディレクトリ `outputs/lerobot_train/output_a` に設定し、さらに `--job_name` や `--policy.device` などのパラメーターも設定しています。また、`--dataset.split` や `--policy.push_to_hub` などのパラメーターのデフォルト値も示しています。この画像は文脈と密接に関連し、学習コマンドのパラメーターがどのように設定されるかを視覚的に示しています。](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![この画像は、学習中のコマンドライン出力を示しています。scheduler、steps、use_policy_training_preset の設定などモデル学習の詳細や、データセット関連のパラメーターが表示されています。また、モデルのパラメーター数や loss などの情報も示されており、たとえば num_total_params が 55917096（52M）で loss が 0.626 です。その下には、`https://download.pytorch.org/models/resnet18-f37072fd.pth` を /home/featurize/.cache/torch/hub/checkpoints ディレクトリへダウンロードするといったファイルのダウンロード情報も表示されています。この画像は文脈で説明されている学習コマンドラインに関連し、学習中にコマンドラインが出力する内容を視覚的に示しています。](../../en/images/d36-02.png)
</column>
</grid>

![この画像は、学習中に生成されたログ情報を示しています。ログには、時刻、学習イテレーション数、loss、学習率など、いくつかの学習ステップが時系列で記録されており、たとえば 2024 年 1 月 14 日の 15:11:53 にイテレーション数が 131k、loss が 0.368 でした。ここで `INFO` はログの種類、`train` は学習フェーズ、`step` はイテレーション数、`loss` は loss 値、`lr` は学習率を表します。この画像は文書内のコマンドラインの注意点のセクションに関連し、学習実行の主要なデータを視覚的に示しています。](../../en/images/d36-03.png)

モデルのアーカイブは約 300 MB です
