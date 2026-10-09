[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | 日本語 | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# モデルを HuggingFace にアップロードする（任意）

## モデルリポジトリを作成する

<grid>
<column width-ratio="0.354197">
![この画像は、HuggingFace のユーザーインターフェースを示しています。アバターアイコンがあり、それをクリックするとドロップダウンメニューが開き、その中の「New Model」オプションが赤い枠で強調されています。この画像は「モデルを HuggingFace にアップロードする（任意）」セクションに関連し、「モデルリポジトリを作成する」ステップに対応して、HuggingFace で新しいモデルを作成する入口を視覚的に示し、プラットフォーム上でモデル関連のリソースを作成する方法をユーザーが理解できるようにしています。](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![この画像は、HuggingFace の Web サイトで新しいモデルリポジトリを作成するインターフェースを示しています。「Owner」ドロップダウンは「TommyZihao」に設定され、「Model name」フィールドには「lerobot_zihao_model_a」、「License」フィールドには「mit」が入力されています。その下には「Base template」オプションと、リポジトリ種別の「Public」および「Private」の選択肢があります。この画像は「モデルリポジトリを作成する」セクションに関連し、モデルリポジトリ作成時に詳細を入力する例を示しています。](../../en/images/d46-02.png)
</column>
</grid>

## モデルリポジトリを確認する

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

現時点では空です

![この画像は、HuggingFace プラットフォーム上の「TommyZihao/lerobot_zihao_model_a」モデルページを示しています。左側にはモデルカードを編集するための「Model card」タブがあります。右側の「Getting started with your model」セクションでは、モデル情報の追加やモデルファイルのプッシュなど、モデルを使い始める方法を説明しています。その下の「Edit Model Card」エリアでは、モデルの License、言語、ベースモデルなどの情報を追加できます。最下部の「Push your model files」エリアでは、CLI、Python、Git、HTTPS、SSH など、モデルファイルをアップロードするいくつかの方法が提供されています。この画像はモデルの HuggingFace へのアップロードに関連し、ページの操作項目を示しています。](../../en/images/d46-03.png)

![この画像は、HuggingFace プラットフォーム上の TommyZihao/lerobot_zihao_model_a モデルリポジトリページを示しています。ページにはモデルのファイルサイズが 1.54 KB、コントリビューターが 1 名、コミット履歴が 1 件で 9 分前であることが表示されています。また、.gitattributes と README.md のファイルが一覧表示され、サイズはそれぞれ 1.52 KB と 24 Bytes で、同じく最初のコミットによるもので、こちらも 9 分前です。この画像はモデルの HuggingFace へのアップロードに関連し、モデルのアップロード後にページがどのように見えるかを示しています。](../../en/images/d46-04.png)

## モデルをアップロードする

以下の内容で `upload_model.py` ファイルを作成します

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

実行します

```Shell
python upload_model.py
```

![この画像は、コマンドラインから `python upload_model.py` コマンドを実行したときの出力を示しています。ファイル処理の進捗が 34%、新しいデータのアップロード進捗も 34% と表示され、`d_model/model.safetensors` と `tokenizer_processor.safetensors` の 2 つのファイルのアップロード進捗がそれぞれ 92% であることが一覧表示されています。この画像はモデルの HuggingFace へのアップロードに関連し、モデルファイルのアップロード時の進捗を視覚的に示しています。](../../en/images/d46-05.png)

## モデルリポジトリを確認する

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![この画像は、TommyZihao の lerobot_zihao_model_a モデルの HuggingFace リポジトリページを示しています。ページにはモデルの License が mit、コントリビューターが 1 名、コミット履歴が 2 件であることが表示されています。中央には README.md、config.json、model.safetensors などいくつかのファイルが一覧表示され、それぞれの右側に「Upload folder using huggingface_hub」というテキストがあり、これらのファイルが huggingface_hub 経由でアップロードされたことを示しています。この画像はモデルの HuggingFace へのアップロードに関連し、モデルファイルが HuggingFace 上でどのように保存されるかを視覚的に示しています。](../../en/images/d46-06.png)

これでモデルファイルが揃いました
