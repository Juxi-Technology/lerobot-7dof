[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | 日本語 | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>データセットを HuggingFace にアップロードする（任意）</title>

# 方法 1：ローカルからアップロードする（推奨しません。アップロード速度が遅い）

- 自動アップロード

データセットを収集するときに `push_to_hub=true` を設定すると、収集が完了すると同時に自動的にアップロードされます

![この画像はコマンドラインインターフェースを示しており、データセットのアップロード処理の実行ログの一部です。上部には SVN や treet W2 などのツールの環境情報が表示されています。中央には「Starting the second pass: moving the mov atom to the beginning of the file」のような処理メッセージがあり、下部には「error messaging the mach port for IMCRunLoopWakeUpReliable」のような実行中のエラーがあり、右側には処理の進捗とデータ転送の数値、たとえば各エントリのデータ量と速度が一覧表示されています。全体として、データセットのアップロード処理中の実行状態の記録を示しています。](../../en/images/d28-01.png)

- 手動アップロード

データセットを収集するときに `push_to_hub=false` を設定し、収集が完了したら手動でアップロードします

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



自動でも手動でも、アップロード速度は非常に遅いです（毎秒約 100 KB）

なぜなら HuggingFace のサーバーは海外にあるからです

# 方法 2：クラウド GPU プラットフォームからアップロードする（推奨）

## クラウド GPU プラットフォーム Featurize にログインする

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## クラウド GPU インスタンスを起動する

## データセットのアーカイブを `Datasets` にアップロードする

## インスタンスのダウンロードコマンドをコピーする

![この画像は Featurize プラットフォームのデータセットページを示しています。上部に「Datasets」というタイトルがあり、その下に「soarm_amazing_hand_pick.zip」という名前のデータセットがあり、サイズは 213.3 MB、16 時間前にアップロードされました。右側には「Cloud Unzip」ボタンがあり、「Like」「Comment」「Copy Instance Download Command」などのボタンもあります。この画像は「データセットのアーカイブを `Datasets` にアップロードする」ステップに関連し、アップロード後のデータセットページを示しています。](../../en/images/d28-02.png)

## クラウド GPU インスタンスのコマンドラインで実行する

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## データセットを HuggingFace にアップロードする

以下の内容で `upload_dataset.py` ファイルを作成します

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

ファイルを実行します

```Shell
python upload_dataset.py
```

![この画像は、クラウド GPU インスタンスのコマンドラインでデータセットのアップロードを実行する様子を示しており、lerobot2 という名前のユーザーが python upload.py コマンドを実行しています。ファイルの処理進捗が表示され、処理対象のファイルは 6 つでいずれも 100% の進捗であり、各ファイルの転送サイズが示され、全体のデータ転送進捗は 100% です。下部には、前回のコミット以降に変更されたファイルがないため、空のコミットを作成しないようにコミットをスキップしたことが記載されています。この内容は upload_dataset.py ファイルを実行するステップに対応します。](../../en/images/d28-03.png)

- もう 1 つのアップロード方法（推奨しません）

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![この画像は HuggingFace の `hf upload` コマンドを使ってデータセットをアップロードする様子を示しており、コマンドは `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset` です。画像ではアップロードが最終段階に入り、すべてのファイルが 100% の進捗になっており、いくつかの動画ファイルと parquet ファイルを含み、そのアップロードサイズは対応するローカルファイルのサイズと正確に一致しています。また、アップロードされた合計ファイルサイズと転送速度、下部にはこのアップロードコミットの HuggingFace データセットページへのリンクがあり、アップロードタスクが完了したことを示しています。](../../en/images/d28-04.png)

# HuggingFace でデータセットを確認する

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![この画像は Hugging Face プラットフォーム上の Juxi-Technology チームの soarm_amazing_hand_pick データセット詳細ページのスクリーンショットで、「HuggingFace でデータセットを確認する」内容に対応しています。上部にはデータセットのナビゲーションオプションと、作者やタグなどの基本情報が表示されています。中央の Dataset Viewer エリアには、データセットの split 1 の学習データの一部が表示され、action、observation_state、timestamp などのフィールドとその値が含まれ、1 レコードのサイズ、総レコード数、合計サイズも示されています。下部には、このデータで学習された関連モデルについても言及されています。](../../en/images/d28-05.png)

![この画像は Hugging Face プラットフォームの soarm_amazing_hand_pick データセットページを示しています。上部には検索ボックスとナビゲーションバーがあり、モデルやデータセットなどを検索できます。データセット情報のセクションには、所有組織 Juxi - Technology と robotics や imitation-learning などのタグが表示されています。「Files and versions」タブの下には data、meta、videos などのフォルダーと README.md ファイルが一覧表示され、アップロード者、アップロード方法、時刻、たとえば「Upload README.md with huggingface_hub」が示されています。この画像は HuggingFace データセットの確認に関連し、データセットのファイルとバージョンを視覚的に示しています。](../../en/images/d28-06.png)
