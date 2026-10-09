[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | 日本語 | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Mac コンピューター

黒い Leader アームは 5V6A 電源アダプターを使用します。

白い Follower アームは 12V5A 電源アダプターを使用します。

## 権限の付与

![Mac のシステム設定ウィンドウの画像で、左サイドバーで「プライバシーとセキュリティ」が選択され、現在は「アクセシビリティ」設定ページが表示されています。ウィンドウには Baidu Netdisk、DingTalk、Doubao などのアプリが一覧表示されており、Terminal アプリのスイッチが赤枠で囲まれ、オンになっています。これは Mac のワークフローにおける「権限の付与」ステップに対応し、後で Miniconda をインストールしたりミラーを変更したりするための準備として、関連する Terminal の権限を有効にするものです。](../../en/images/d04-01.png)

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
conda create -y -n lerobot python=3.12
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

![`ffmpeg` コマンドを実行した後のターミナル出力の画像で、ffmpeg が正常にインストールされたことを確認するために使われ、「ffmpeg のインストール」後の確認ステップに対応します。出力には FFmpeg バージョン 7.1.1、2000 年から 2025 年までの FFmpeg 開発者による著作権、構成フラグ、対応するエンコーダーの一覧、Universal Media Converter の使い方の説明がはっきりと表示され、最後にさらに詳しいヘルプについては `-h` オプションまたは `man ffmpeg` コマンドを使うよう案内しています。](../../en/images/d04-02.png)

## LeRobot のダウンロード

- 公式 LeRobot リポジトリをダウンロードする

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## コードリポジトリのインストール

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![Mac で pip を使って「feetech」パッケージをインストールしているコマンドラインの画像です。インストールの進捗が表示され、「https://repo.huaweicloud.com/repository/pypi/simple/」からパッケージを取得し、datasets、diffusers、huggingface-hub などのファイルをダウンロードし、最後に「einops==0.8.0」のダウンロードで終わっています。この画像は「コードリポジトリのインストール」セクションに関連し、リポジトリをインストールするときのコマンド実行と結果を視覚的に示しています。](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![Mac で LeRobot コードリポジトリをインストールした後の確認画面の画像です。ターミナルには「Successfully installed LeRobot」と表示され、numpy や pandas など、インストールされた複数の Python パッケージとそのバージョンが一覧表示されています。この画像は「コードリポジトリのインストール」と「インストールの確認」のセクションに対応し、インストールが成功したことを確認できるよう、インストールされたパッケージを視覚的に示しています。](../../en/images/d04-04.png)
</column>
</grid>

## インストールの確認

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![これは Mac のターミナルコマンドラインのスクリーンショットで、LeRobot インストール手順における設定確認の一部です。現在のプロジェクトディレクトリ lerobot-main が表示され、python コマンドの後に darwin システム上の Python バージョン 3.10.19 の対話型 Python 環境に入っています。import lerobot、import torch、torch.cuda.is_available() のコマンドが順に実行され、CUDA の可用性は False と表示され、続いて import scservo_sdk が実行されています。これは確認ステップに対応し、LeRobot とその依存関係のインストールおよび設定状況を確認するために使われます。](../../en/images/d04-05.png)

![Mac での LeRobot コマンドのターミナル出力の画像です。LeRobot バージョン 0.4.3、プラットフォーム macOS - 15.6.1 - arm64 - arm - 64bit、Python バージョン 3.12.12 などの情報が表示されています。また、Huggingface Hub、Datasets、NumPy、FFmpeg、PyTorch のバージョン情報、PyTorch が CUDA サポート付きでビルドされているか、CUDA バージョンと GPU モデル、最後に LeRobot スクリプトの一覧も表示されています。この画像は確認の文脈に対応し、インストール後の LeRobot 情報を示しています。](../../en/images/d04-06.png)
