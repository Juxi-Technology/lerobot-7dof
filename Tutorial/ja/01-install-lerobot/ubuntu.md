[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | 日本語 | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Ubuntu コンピューター

黒い Leader アームは 5V6A 電源アダプターを使用します。

白い Follower アームは 12V5A 電源アダプターを使用します。

## Miniconda のインストール

https://www.anaconda.com/download

## pip ミラーの変更

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## conda ミラーの変更

```Shell
# 既存の .condarc 設定をクリアする（任意、競合を避けるため）
echo "" > ~/.condarc

# 清華ミラーの設定を書き込む
cat << EOF > ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
EOF

# 設定を反映させるためキャッシュをクリアする
conda clean -i
```

## 仮想環境の作成

```Shell
conda create -y -n lerobot python=3.12 -y
```

## 仮想環境の有効化

```Shell
conda activate lerobot
```

## ffmpeg のインストール

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

インストールが成功したか確認します

```Shell
ffmpeg
```

<grid>
<column width-ratio="0.568354">
![Ubuntu コンピューターで conda の仮想環境を有効化し、ffmpeg をインストールした結果の画像です。まず conda が lerobot という名前の仮想環境を有効化し、次に conda install ffmpeg=7.1.1 -c conda -forge コマンドを実行して、conda - forge を含む conda の Channels 情報を表示し、最後に linux - 64 の Platform と、完了した Collecting package metadata および Solving environment の処理を示しています。この画像は「ffmpeg のインストール」セクションに対応し、インストールの実行を視覚的に示しています。](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![これは Ubuntu システムのターミナルのスクリーンショットで、ffmpeg コマンドを実行した後に返された結果を示しています。ffmpeg バージョン 7.1.1、その設定情報、対応するモジュール（libavcodec や libavformat など）のバージョン番号、および汎用メディアコンバーターの使い方の説明が表示されています。これは ffmpeg をインストールした後の確認ステップに対応し、ffmpeg ツールがシステムに正常にインストールされたことを確認するために使われます。](../../en/images/d03-02.png)
</column>
</grid>

## 公式 LeRobot リポジトリのダウンロード

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## コードリポジトリのインストール

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![この画像は、Ubuntu システムのターミナルで lerobot ディレクトリに入り、公式 LeRobot リポジトリのインストールの一部である `pip install -e.\[feetech\]` コマンドを実行する様子を示しています。指定された Huawei Cloud リポジトリからのパッケージ取得、依存関係のインストール、関連するデータセットパッケージ（diffusers、huggingface-hub、accelerate など）のダウンロードを含め、コマンド実行の各ステップがはっきりと表示されており、いくつかのパッケージには具体的なダウンロード進捗、サイズ、速度が示され、最後に依存関係がすでに満たされているというメッセージでリポジトリのインストールが完了しています。](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![この画像は、Ubuntu システムのターミナルでソフトウェアパッケージをインストールする際のコマンドラインインターフェースを示しており、ffmpeg などのソフトウェアをインストールするときの依存関係の処理情報を含んでいます。ターミナルには、pytz、pyyaml、numpy など処理中のパッケージの一覧や、既存のパッケージバージョンをアンインストールして新しいものをインストールする過程、依存関係の整合性に関する注記が表示されています。この内容は「ffmpeg のインストール」の後の「インストールの確認」ステップに対応し、ffmpeg などのソフトウェアのインストール過程を確認したときのターミナル出力の記録です。](../../en/images/d03-03.png)
</column>
</grid>

## インストールの確認

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## 4090 ホストでの実行結果

![Ubuntu コンピューターで LeRobot コマンドと情報を実行している画像です。「Lerobot lerobot -info」コマンドは LeRobot バージョン 0.4.3、プラットフォーム Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35、Python バージョン 3.12.0 などの情報を表示しています。そのうち PyTorch バージョンは 2.7.1 + cu126、CUDA バージョンは 12.6、GPU モデルは NVIDIA GeForce RTX 4090 です。この画像はインストール成功の確認に関連し、Ubuntu 環境における LeRobot の動作情報を示しています。](../../en/images/d03-04.png)

![Ubuntu コンピューターで LeRobot リポジトリを実行しているときの Python 対話インターフェースの画像です。Python バージョン 3.10.12 が表示され、conda-forge のパッケージ情報やコンパイル時刻も含まれています。ユーザーはコマンド `import lerobot`、`lerobot.__version__`、`import torch`、`torch.cuda.is_available()`、`import scservo_sdk` を順に入力し、LeRobot のバージョン番号 0.4.3、CUDA の可用性 True、`scservo_sdk` のインポート成功を得ています。この画像は LeRobot リポジトリのインストール確認に関連し、確認プロセスを視覚的に示しています。](../../en/images/d03-05.png)

## NVIDIA DGX Spark での実行結果

![Ubuntu コンピューターで LeRobot リポジトリを実行しているときのターミナル出力の画像です。LeRobot バージョン 0.4.4、CUDA バージョン 13.0、GPU モデル NVIDIA GeForce GTX 1660 Ti が表示されています。また、HuggingFace Hub、Datasets、PyTorch などのライブラリのバージョンや、FFmpeg と PyTorch のツールのバージョンも一覧表示されています。最後に LeRobot と torch のインポートを確認しており、torch.cuda.is_available() は True を返し、CUDA が利用可能であることを示しています。この画像は Ubuntu コンピューターでの LeRobot リポジトリの実行に関連し、その結果を示しています。](../../en/images/d03-06.png)
