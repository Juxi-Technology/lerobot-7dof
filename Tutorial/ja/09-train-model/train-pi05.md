[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | 日本語 | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# 学習コマンドライン - pi0.5

## 参考ドキュメント

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## 推奨するクラウド GPU インスタンス

![この画像は、Alibaba Cloud が提供する RTX A6000 クラウド GPU インスタンスの詳細を示しています。従量課金の価格は 3.29 元/時間で、3 枚が利用可能です。このインスタンスは RTX A6000 GPU（GPU メモリの合計は 51.0 GB）、30 コアの AMD EPYC 7742 CPU、メモリ 60.9 GB、ディスク 429.5 GB で構成されています。下部には青い「Start Using」ボタンがあります。この画像は「推奨するクラウド GPU インスタンス」セクションに関連し、推奨するクラウド GPU の構成、価格などの主要情報を視覚的に示しています。](../../en/images/d40-01.png)

## 環境のインストール

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## コマンドライン

- 前回中断した学習で output 配下に残ったファイルを削除する

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- 学習

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi05 \
    --output_dir=~/output_lerobot_train/shake/pi05_A \
    --job_name=shake_pi05_A \
    --policy.pretrained_path=lerobot/pi05_base \
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

<grid>
<column width-ratio="0.468128">
![この画像は、コマンドラインから身体知能（PI）モデルを学習するときの出力を示しています。モデルがロードされ、パラメーターが再マッピングされ、オプティマイザーとスケジューラーが作成される様子が表示されており、たとえば「Loading model from: lerobot/pi05_base」などがあります。また、「Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy」のような警告メッセージも表示されています。さらに「num_total_frames: 180K」といった学習関連の数値も示されています。この画像は学習コマンドラインの操作に関連し、学習実行の主要な情報を視覚的に示しています。](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![この画像は、コマンドラインでの学習中に表示される出力を示しています。学習中に huggingface/torch プロセスがフォークされますが、すでに並列処理が使われているため、デッドロックを避けるために並列処理が無効化され、「できるだけフォークの前に」行わないよう促すメッセージが表示されます。学習は 20 分ほど経ってからようやく本格的に始まります。この画像は文脈と密接に関連し、学習中に現れる可能性のあるプロセス操作と注意メッセージを示すことで、状態と注意点を説明するのに役立っています。](../../en/images/d40-02.png)
</column>
</grid>

コマンドラインを実行してから 20 分ほど経つと、ようやく学習が本格的に始まります

モデルのアーカイブは約 5 GB で、解凍すると約 7 GB になります
