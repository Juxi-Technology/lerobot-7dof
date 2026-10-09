[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | [Deutsch](../de/09-train-model/train-pi0.md) | [Español](../es/09-train-model/train-pi0.md) | [Français](../fr/09-train-model/train-pi0.md) | [Italiano](../it/09-train-model/train-pi0.md) | 日本語 | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# 学習コマンドライン - pi0（最良の結果）

## 参考ドキュメント

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## 推奨するクラウド GPU インスタンス

![この画像は、RTX A6000 クラウド GPU インスタンスの詳細を示しています。従量課金の価格は 3.29 元/時間、GPU は RTX A6000 で GPU メモリの合計は 51.0 GB、CPU は 30 コアの AMD EPYC 7742、メモリは 60.9 GB、ディスクは 429.5 GB です。右上には 3 枚が利用可能であることが示されています。下部には青い「Start Using」ボタンがあります。この画像は「推奨するクラウド GPU インスタンス」セクションに関連し、推奨するクラウド GPU の構成、価格などの主要情報を視覚的に示しています。](../../en/images/d39-01.png)

## 環境のインストール

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## コマンドライン

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_A

lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0 \
  --output_dir=~/output_lerobot_train/shake/pi0_A \
  --job_name=shake_pi0_A \
  --policy.pretrained_path=lerobot/pi0_base \
  --policy.compile_model=true \
  --policy.gradient_checkpointing=true \
  --policy.dtype=bfloat16 \
  --policy.freeze_vision_encoder=false \
  --policy.train_expert_only=false \
  --steps=50000 \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --batch_size=8
```

コマンドラインを実行してから 20 分ほど経つと、ようやく学習が本格的に始まります

モデルのアーカイブは約 5 GB で、解凍すると約 7 GB になります
