[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | [Deutsch](../de/10-inference/inference-dgx-spark.md) | [Español](../es/10-inference/inference-dgx-spark.md) | [Français](../fr/10-inference/inference-dgx-spark.md) | [Italiano](../it/10-inference/inference-dgx-spark.md) | 日本語 | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# NVIDIA DGX Spark での推論

## 環境のインストール

- PyTorch

PyTorch は公式サイトから別途インストールし、CUDA 13.0 版を使用します

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![この画像は、ターミナルで LeRobot 環境をインストールするコマンドと結果を示しています。まず「pip install -e /Downloads/lerobot」コマンドを実行し、次に「python -m lrobot -h」を実行して LeRobot のヘルプ情報を表示し、LeRobot のバージョンが 0.4.4 であることを確認しています。最後に「pip show lrobot」を実行し、LeRobot の作者やホームページなどの情報を表示しています。この画像は LeRobot 環境のインストールに関連し、インストール手順とその結果を示しています。](../../en/images/d56-01.png)

- 次に pyproject.toml ファイル内の torch を単独でコメントアウトする

![この画像は pyproject.toml ファイルの内容を示しており、torchcode="2.3.0, c2.8.0" の行が赤い枠で強調されています。このファイルはプロジェクトの依存関係を指定するために使われる Python プロジェクトの構成ファイルです。文脈では、pyproject.toml ファイル内の torch を単独でコメントアウトし、その後 pip install -e を実行することに触れており、この画像はその文脈に関連して、次のステップの参考として pyproject.toml ファイル内の torchcode の位置を視覚的に示しています。](../../en/images/d56-02.png)

次に pip install -e . を実行します

- 注意：推論コマンドラインでは、policy.path のパスを Spark 内の実際のモデルパスに変更してください

## 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo chmod 666 /dev/ttyACM*
```



```Shell
sudo rm -rf /home/apx103/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

## ACT

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/ACT/5K/pretrained_model
```

## SmolVLA

- 環境のインストール

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- 推論

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## WALL-OSS

- 環境のインストール

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- コードの追加

![この画像は、lerobot プロジェクトの policies フォルダー内の factory.py ファイルの一部を示しています。赤い枠内の重要なコードは「from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors」で、続けて「processors = make」などの文があります。この画像は pi0 モデルの推論セクションに関連し、pi0 の推論操作を完了するためにこのコードを追加する必要があることを説明しており、pi0 推論コードの重要な部分です。](../../en/images/d56-03.png)

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

## pi0

- 環境のインストール

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- 推論

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```
