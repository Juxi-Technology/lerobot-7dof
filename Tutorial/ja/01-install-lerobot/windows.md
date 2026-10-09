[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | 日本語 | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Windows コンピューター

黒い Leader アームは 5V6A 電源アダプターを使用します。

白い Follower アームは 12V5A 電源アダプターを使用します。

## Miniconda のインストール

anaconda.com/download/success

または、このリンクをクリックしてインストーラーを直接ダウンロードします

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![これは Windows での Miniconda3 インストール画面の画像で、ソフトウェアバージョン py312_24.7.1-0（64-bit）を示しています。画面には 2 つのインストールタイプの選択肢があり、「Just Me (recommended)」と書かれた選択肢が赤枠で強調され、現在選択されている推奨のインストール方法になっており、もう一方の「All Users (requires admin privileges)」は選択されていません。画面上部では Miniconda3 のインストールタイプを選ぶよう促しており、下部には「Back」「Next」「Cancel」の 3 つのボタンがあります。この画面は Miniconda のインストールフローにおいて、インストール範囲を確定する重要なステップです。](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![この画像は Miniconda3 インストール画面の詳細オプションを示しています。「Add Miniconda3 to my PATH environment variable」という選択肢が赤枠で強調され、その横に、他のアプリケーションと競合する可能性があるため推奨されないという説明と、代わりに Windows のスタートメニューに追加されるコマンドプロンプトと PowerShell のメニューを使うよう勧める注記があります。この画像は「conda ミラーの変更」の後に仮想環境を作成するステップに関連し、Miniconda をインストールするときの設定の参考になります。](../../en/images/d05-02.png)
</column>
</grid>

## conda ミラーの変更

```Shell
# まず既存のミラー設定をクリアする（競合を避けるため）
conda config --remove-key channels

# conda のデフォルトチャンネルと一般的なサードパーティチャンネルを清華ミラーに置き換える
# デフォルトのパッケージチャンネル（main/r/msys2）を追加する
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# 一般的なサードパーティチャンネルを追加する
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# ダウンロード元の表示を有効にして、パッケージのインストール時に実際のダウンロード先が表示されるようにする
conda config --set show_channel_urls yes

# 新しいミラーを反映させるためインデックスキャッシュをクリアする
conda clean -i

# 現在の設定を表示する（チャンネルが正しく追加されたか確認するため）
conda config --show-sources
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

![これは Windows のコマンドラインウィンドウの画像で、ffmpeg コマンドを実行した後の確認結果を示しています。具体的には、コマンドラインに ffmpeg バージョン 7.1.1、著作権とビルド情報、ffmpeg に関連するライブラリファイルの情報が出力され、下部には基本的な使い方とさらに詳しいヘルプの取得方法を説明する使用上の注意が表示されています。この画像は、Windows コンピューターに ffmpeg が正常にインストールされたことを確認するために使われ、「ffmpeg のインストール」後の確認ステップに対応し、ffmpeg のインストール完了後の動作状態を視覚的に示しています。](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![これは Linux のターミナルインターフェースで、conda 関連のコマンドとその実行を示しています。2 つの中核的なコマンドがはっきりと示されています。すなわち lerobot という名前の仮想環境を有効化するコマンド「$ conda activate lerobot」と、有効な環境を無効化するコマンド「$ conda deactivate」です。現在は (base) 環境が有効になっており、ターミナルは conda-forge チャンネルから ffmpeg 7.1.1 をインストールする処理を実行しており、設定された複数のミラーアドレスが表示され、パッケージメタデータと依存環境の収集フローはすでに完了しています。](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![この画像は Ubuntu で ffmpeg コマンドを使ったときのターミナルインターフェースを示しています。ffmpeg のバージョン情報として、バージョン番号、ビルダー、コンパイラの設定詳細が表示されています。また、libavcodec や libavformat など各種コーデックのバージョンも一覧表示されています。下部には使用方法の説明があり、すべてのヘルプには「-h」を、または「man ffmpeg」を実行するよう促しています。この画像は「ffmpeg のインストール」セクションに関連し、バージョンとビルド情報を示すことで ffmpeg のインストール成功を確認するために使われます。](../../en/images/d05-05.png)
</column>
</grid>

## LeRobot のダウンロード

- 公式 LeRobot リポジトリをダウンロードする

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## コードリポジトリのインストール

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![この画像は、LeRobot コードリポジトリをインストールした後の Windows の cmd コマンドラインでの確認結果を示しています。コマンドラインには「Successfully built lerobot」などのメッセージが表示され、インストールが成功したことを示しています。また、numpy 1.22.3 や scipy 1.7.1 など、いくつかの Python パッケージとそのバージョン番号が一覧表示されています。下部にはプロンプト「(lerobot) C:\\Users\\40743\\Downloads\\lerobot>」が表示され、現在のディレクトリが Downloads フォルダー内の lerobot フォルダーであることを示しています。この画像は「インストールの確認」セクションに対応し、インストール成功後のコマンドラインフィードバックを視覚的に示しています。](../../en/images/d05-06.png)

## インストールの確認

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![この画像は、Windows の Python 環境でインストールの成功を確認している画面を示しています。コマンドラインには Python バージョン 3.10.19 が表示され、lerobot、scservo_sdk、torch などのモジュールをインポートするコードが実行され、最後に torch.cuda.is_available() を実行して False が返されています。この画像は「インストールの確認」セクションに対応し、LeRobot コードリポジトリをインストールした後に Python 環境を通じてインストール成功を確認する操作と結果を視覚的に示しています。](../../en/images/d05-07.png)
