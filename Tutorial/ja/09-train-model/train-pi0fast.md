[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | [Deutsch](../de/09-train-model/train-pi0fast.md) | [Español](../es/09-train-model/train-pi0fast.md) | [Français](../fr/09-train-model/train-pi0fast.md) | [Italiano](../it/09-train-model/train-pi0fast.md) | 日本語 | [한국어](../ko/09-train-model/train-pi0fast.md) | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# 学習コマンドライン - pi0fast

## 参考ドキュメント

https://huggingface.co/docs/lerobot/pi0fast

## 課題

https://github.com/huggingface/lerobot/pull/2203

## 推奨するクラウド GPU インスタンス

![この画像は、推奨する RTX A6000 クラウド GPU インスタンスの詳細を示しています。3 枚が利用可能で、従量課金の価格は 3.29 元/時間であることが示されています。構成は RTX A6000 GPU（GPU メモリの合計は 51.0 GB）、30 コアの AMD EPYC 7742 CPU、メモリ 60.9 GB、ディスク 429.5 GB で、下部に「Start Using」ボタンがあります。この画像は「推奨するクラウド GPU インスタンス」セクションにあり、学習のためのクラウド GPU の推奨と、その主要な構成および価格をユーザーに示しています。](../../en/images/d41-01.png)

## 環境のインストール

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## コマンドライン

- 前回中断した学習で output 配下に残ったファイルを削除する

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- 学習

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi0_fast \
    --output_dir=output_lerobot_train/shake/pi0_fast_A \
    --job_name=shake_pi0_fast_A \
    --policy.pretrained_path=lerobot/pi0_fast_base \
    --policy.dtype=bfloat16 \
    --policy.gradient_checkpointing=true \
    --policy.chunk_size=10 \
    --policy.n_action_steps=10 \
    --policy.max_action_tokens=256 \
    --steps=50000 \
    --batch_size=8 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project
```





## 以前の内容

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0_fast \
  --output_dir=output_lerobot_train/shake/pi0_fast_A \
  --job_name=shake_pi0_fast_A \
  --policy.pretrained_path=lerobot/pi0_fast_base \
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









実行してから、学習が本格的に始まるまでに 10 分ほどかかります

モデルのアーカイブは約 5 GB で、解凍すると約 7 GB になります
