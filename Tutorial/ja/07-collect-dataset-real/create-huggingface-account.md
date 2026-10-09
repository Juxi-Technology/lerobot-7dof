[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | 日本語 | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Hugging Face アカウントの登録（任意）

## 中国国内向けの HuggingFace ミラーを設定する

- Ubuntu

```Shell
sudo nano ~/.bashrc

# ファイルの末尾にこれを追加する
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# 出力
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# ファイルの末尾にこれを追加する
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# 出力
# https://hf-mirror.com
```



## トークンの作成

https://huggingface.co/settings/tokens

![この画像は Hugging Face サイトの「トークン設定」ページを示しています。ページの左側にはユーザーのアバターと個人情報があり、右側は「ユーザーアクセストークン」の領域で、アクセストークンは認証に用いられ、トークンの権限に基づいてアプリケーションが操作を実行できるようにすること、そしてトークンを他人と共有しないことを強調しています。ページの右上には「+ Create new token」ボタンがあり、新しいトークンを作成するために使います。この画像は、文書の「トークンの作成」の内容に関連し、トークン作成の操作入口を視覚的に示しています。](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![この画像は、Hugging Face プラットフォームで新しいアクセス資格情報（トークン）を作成するページを示しています。ページの「Token type」領域では「Write」オプションが強調され、同時に「Token name」入力欄には「lerobot_zihao_token」が入力されています。ページの下部には作成操作を送信する「Create token」ボタンがあり、ページの左側には現在ログインしているユーザーのアカウント情報が表示されています。](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![この画像は、Hugging Face サイトの「Save your Access Token」ポップアップ画面を示しています。ポップアップは、token 値を安全な場所に保存するよう促し、閉じると再び表示できなくなります。下部には token 値「<your-huggingface-token>」が表示され、「Copy」ボタンが token をコピーするために用意されています。さらに下部には token 名「lerobot_zihao_token」と権限「WRITE」があります。この画像は、文書の「トークンの作成」ステップに関連し、生成した Token を記録するようユーザーを導くために用いられます。](../../en/images/d24-03.png)
</column>
</grid>

## トークンの記録

たとえば、私のものは次のとおりです：

```Shell
<your-huggingface-token>
```

## トークンの連携

```Shell
hf auth login

hf auth whoami
```

![この画像は、Mac システムでコマンドラインを使って Hugging Face アカウントを操作する画面を示しています。まず「hf auth login」コマンドを実行すると、既存の token があり、「hf auth whoami」で情報を取得するか「hf auth logout」でログアウトできることが示されます。次に token を入力し、確認後に token が指定されたパスに保存され、現在アクティブな token が「lerobot zihao token」であることが表示されます。最後に「hf auth whoami」コマンドを実行すると、ユーザーが「TommyZihao」であることが表示されます。この画像は、文書の「トークンの作成」部分に対応し、Token 作成の操作と結果を視覚的に示しています。](../../en/images/d24-04.png)

## データセットリポジトリの作成

<grid>
<column width-ratio="0.480604">
![これは Hugging Face プラットフォームのインターフェースのスクリーンショットで、ページの左上には Community、Docs、Pricing などの機能入口があり、個人アカウント TommyZihao の情報は画面中央の領域にあります。緑色の枠で示されているのは、ページ右上のユーザーアバターアイコンです。画面中央のメニューでは、赤い枠で「+ New Dataset」オプションが強調されており、このオプションは文脈の「データセットリポジトリの作成」の操作ステップに対応し、これをクリックするとデータセットリポジトリの作成を開始できます。](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![この画像は、Hugging Face で新しいデータセットリポジトリを作成するインターフェースを示しています。「Dataset name」には「lerobot_zihao_dataset_a」が入力され、「License」は「mit」が選択され、「Public」オプションにチェックが入っています。下部には「Create dataset」ボタンがあります。この画像は、文書の「データセットリポジトリの作成」の内容に対応し、データセットリポジトリ作成時に入力すべき重要事項を視覚的に示し、Hugging Face でデータセットリポジトリを作成する操作インターフェースと入力すべきフィールドの理解を助けます。](../../en/images/d24-06.png)
</column>
</grid>

![これは Hugging Face プラットフォームの Web ページで、ユーザー TommyZhao が作成した lexobot_zihao_dataset_a という名前のデータセットリポジトリの内容を、Dark テーマモードで表示しています。ページの左側には「Edit Dataset Card」の編集オプションがあり、データセットファイルをプッシュするための複数の操作方法（CLI、Python、HTTPS、SSH の関連コマンド例を含む）も提供されています。また、このデータセットの License が mit であることも示されています。](../../en/images/d24-07.png)
