[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | 日本語 | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# クラウド GPU 学習環境の構成

## お使いのコンピューターのネットワークプロキシを無効にする

そうしないと、Jupyter のコマンドラインを開けない場合があります

## クラウド GPU プラットフォーム Featurize にログインする

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

ユーザーグループに加入し、カスタマーサポートに「同济子豪兄」のファンであると伝えると、クーポンを受け取れます

## クラウド GPU インスタンスを起動する

## 環境のインストールと構成

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## wandb にログインする

```Shell
wandb login
API Key をコピーして貼り付け、Enter を押します
```

![この画像は、Lerobot 環境で `wandb login` コマンドを使って W&B（Weights & Biases）にログインする過程を示しています。コマンドを実行すると、W&B は https://api.wandb.ai へのログインを促し、ローカルの W&B サーバーをデプロイできること、API キーの探し方を案内します。続いて、W&B は API キーを貼り付けて Enter を押すよう促し、.netrc ファイルが見つからず作成中であることも示します。最後に、W&B は tommyzihao としてログイン済みであることと、ログイン状態が https://api.wandb.ai であることを表示します。この画像は、文書の「wandb にログインする」ステップに関連し、ログイン操作のフィードバック情報を視覚的に示しています。](../../en/images/d35-01.png)

## データセットをマウントする

```Shell
インスタンスのダウンロードコマンドをコピーします。たとえば：
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

データセットは `~` ディレクトリの下に現れます

## 重みの保存頻度を調整する（任意）

`lerobot/src/lerobot/configs/train.py` を開きます

save_freq を 20_000 から 5_000 に変更します

こうすると、学習のより早い段階でモデルの重みファイルを得られます
