[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | 日本語 | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# D-Robotics RDK S100 での推論

詳細な実装フローについては、こちらのリンクを参照してください<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy 全ワークフロー文書" type="doc"></cite>



## RDK S100/S100P での ACT モデルのエンドツーエンドデプロイ

このセクションでは、D-Robotics RDK S100 シリーズのハードウェア上で ACT モデルをデプロイする一連の流れを最後まで解説します。全体は 3 つの中心的な段階で構成されます。すなわち **モデルのエクスポート**、**量子化コンパイル**、**ボード上での実行** です。

<callout emoji="💡">
**前提条件：**
- **開発マシン (Host)：**ステップ 1 とステップ 2 を実行するために使用します。通常はモデルの学習に使用したマシンです（相応の性能があり、Docker がインストールされている必要があります）。
- **ボード (Edge)：**D-Robotics RDK S100/S100P で、ステップ 3 を実行するために使用します。
- **ツールチェーン：**本記事は `rdk_LeRobot_tools` リポジトリに依存しています。詳細は [GitHub リポジトリ](https://github.com/D-Robotics/rdk_LeRobot_tools)を参照してください。
</callout>

<callout emoji="🚨">
**バージョン互換性に関する重要な注意（必読）：** 現在の `rdk_LeRobot_tools` の ONNX エクスポートフローは **LeRobot datasets v2.1** と完全に互換です。最新の v3.0 ではデータ構造が変更されているため、本セクションの作業を行う前に、元の `lerobot` メインリポジトリを v2.1 と互換の特定の commit に切り替えて、エクスポートフローが円滑に進むようにすることを **強く推奨** します。
*推奨する Commit ID：* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### ステージ 1：モデルを ONNX 形式へエクスポート 💻（開発マシン上）

まず、**PyTorch で学習した** モデルを中間形式（ONNX）へエクスポートする必要があります。



#### **1. ツールチェーンリポジトリをクローンする** 

`lerobot` の作業ディレクトリに移動し、RDK 専用のツールチェーンをクローンします：

```Bash
cd lerobot

# 1. v2.1 データセットと互換の安定版に切り替える
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. D-Robotics RDK 専用のツールチェーンをクローンする
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. エクスポートパラメーターを構成する** 

`rdk_LeRobot_tools/bpu_export_config.yaml` ファイルを編集し、実際のパスに合わせて構成を調整します：

```YAML
dataset:
  root: "data/so101_pick_place" # データセットへの絶対パスまたは相対パス
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # 元の PyTorch モデルの重みへのパス
type: "nash-e" # 対象ハードウェアアーキテクチャ。RDK S100 は nash-e、S100P は nash-m に対応
```



#### 3. エクスポートスクリプトを実行する

```Bash
# ONNX をエクスポートする（開発マシン）
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **成功の目安**：カレントディレクトリに `bpu_export_output` フォルダーが作成され、その中に `build_all.sh` スクリプトと、後で必要になる量子化キャリブレーションデータが含まれています。



### ステージ 2：BPU モデルをコンパイルする 🐳（開発マシン上の Docker 環境で）

D-Robotics BPU モデルの量子化とコンパイルには OpenExplorer（OE）環境が必要です。環境を分離するために Docker の使用を推奨します。



#### **1.** **Docker 環境とイメージを準備する** 

開発マシンに Docker がインストールされていることを確認してください（[公式インストールガイド](https://docs.docker.com/engine/install/)）。推奨する CPU イメージをダウンロードしてロードします：

```Bash
# ダウンロードしたオフラインイメージアーカイブをロードする
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. コンパイル用コンテナを起動する**

<callout emoji="⚠️">
**注意喚起**：モデルのコンパイルには大量の共有メモリが必要です。必ず `--shm-size=15g` 引数を追加してください。そうしないと IPC メモリエラーが発生しやすくなります。
</callout>

開発マシンの作業ディレクトリ（先ほどエクスポートしたフォルダーを含む）をコンテナにマウントします：

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

（注：`<docker-image-name>` は、`sudo docker images` で確認できる実際のイメージ名に置き換えてください。）



#### **3.** **コンテナ内でコンパイルを実行する** 

コンテナ内に入ったら、ワンクリックのコンパイルスクリプトを実行します：

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **ビルド成果物を確認する** 

コンパイルが完了すると、`bpu_export_output` の下に `bpu_output/` フォルダーが作成されます。その中には、RDK ボード上で実行するために必要なすべての中核ファイルが含まれています：

- `bpu_output/` ディレクトリ構造を表示するにはクリック

  - `BPU_ACTPolicy_TransformerLayers.hbm`（量子化モデルファイル）
  - `BPU_ACTPolicy_VisionEncoder.hbm`（量子化モデルファイル）
  - `action_mean.npy` などのデータセット正規化パラメーター
  - `camera1_mean.npy` などのカメラ統計パラメーター

---

### ステージ 3：ボード上でのデプロイと推論 🤖（RDK S100 上）

<callout emoji="📌">
**前提条件の確認：**
1. RDK ボードにはすでに `D-Robotics/lerobot` の実行環境が構成され、`hbm_runtime` がインストールされている。
2. 前のステップで生成した `bpu_output/` フォルダー全体が、`scp` や USB メモリーなどを用いて RDK ボードへ完全にコピーされている。
3. 基本的なテレオペレーションの構成が済んでおり、アームのシリアルポート、カメラの USB ポート、キャリブレーションファイルが正しく構成されている。
</callout>



#### **1.** **BPU で高速化した推論を実行する**

RDK ボードのターミナルで、ツールチェーンのディレクトリへ移動し、制御スクリプトを起動します：

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ トラブルシューティング

実際のデプロイで問題に直面した場合は、次の一覧と照らし合わせて確認してください：

- **アームが動かない？**

  - デバイスがマウントされているか確認します。ターミナルで `ls /dev/ttyACM*` と入力し、アームのシリアルポートが正しいことを確認してください。
  - 権限を確認します。推論スクリプトを `sudo` で実行してみるか、現在のユーザーを `dialout` グループに追加してください。
- **カメラのストリーミングがエラーになる／画像が異常／アームがその場で震える？**

  - ホットプラグによってカメラのインデックスがずれていないか確認し、コード内のカメラパラメーターが実際の `/dev/video*` と一致しているか確認してください。
- **開発マシンでコンテナが生成したファイルをコピーすると「権限が不足しています」と表示される？**

  - Docker でマウントしたディレクトリで作成されたファイルは、デフォルトで root が所有します。開発マシンで `sudo chown -R $USER:$USER bpu_export_output` を実行すると解決します。
