[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | 日本語 | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# 推論コマンドライン - pi0

## Ubuntu

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- 推論コマンドライン

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![この画像は、Ubuntu 環境で SSH 経由でマシンに接続するときに現れるエラーを示しています。プラットフォームがサポートされていないこと、X 接続を確立できないこと、X サーバーが実行されているか、DISPLAY 環境変数が正しく設定されているかを確認するよう促すコンソールエラーが表示されています。また、ヘッドレス環境に関する警告と、エピソード 0 が記録された記録も表示されています。この画像は Ubuntu の推論コマンドラインに関連し、操作中に遭遇する異常な状況である可能性があります。](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![この画像は、Ubuntu 環境で推論コマンドラインを実行したときの出力を示しています。実行中に「E0119」エラーメッセージが何度か現れ、オートチューニング中に有効な triton 構成がなく、共有メモリ不足などリソースを使い果たしたことが示されています。また、ALLOW_TF32、BLOCK_K、BLOCK_M などいくつかの triton_mm モデルの実行時パラメーターと、対応する ACC_TYPE、ALLOW_TF32、BLOCK_K、BLOCK_M の値が示されています。この画像は Ubuntu の推論コマンドラインに関連し、実行中に遭遇したリソース不足を示しています。](../../en/images/d51-02.png)
</column>
</grid>

![この画像は、Ubuntu 環境での推論コマンドラインセッション中のターミナルを示しています。triton_mm 命令の実行結果がいくつか表示され、たとえば triton_mm_3644 は 0.2355 ms かかり、いずれも t1.float32 型で ALLOW_TF32=True を使用しており、BLOCK_K などのパラメーターも示されています。最後には SingleProcess AUTOTUNE のベンチマークが 0.7305 秒、20 個の選択肢のプリコンパイルが 0.0001 秒かかることが示されています。この画像は Ubuntu の推論コマンドラインに関連し、実際の実行状況を示しています。](../../en/images/d51-03.png)

> **動画は準備中**：原文にはここに `VID_20260120_182109.mp4`（元は 310 MB）が埋め込まれています。飛書側ではこのファイルについてダウンロード可能な動画ストリームが提供されておらず、メタデータのみだったため、取得できませんでした。閲覧するには[原文ドキュメント](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg)を参照してください。



## Mac

- 既存の eval という接頭辞のデータセットがあれば削除する

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推論コマンドライン

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![この画像は、Ubuntu 環境での推論コマンドラインセッション（11 - yolo26）中のターミナルを示しています。Python 3.12 のバージョン情報と、robot-type が follower に設定された記録が表示されています。また、color_mode、fourcc、fps、height、width などカメラ関連のパラメーターが一覧表示され、モデルが読み込まれるパスと、モデルの読み込みエラーなどいくつかの警告メッセージが示されています。この画像は Ubuntu の推論コマンドラインに関連し、操作中のターミナルのフィードバックを示しています。](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![この画像は、Ubuntu 環境で Python コードによる推論を実行したときのコマンドライン出力を示しています。「PIBPytorch model」が正常に読み込まれたこと、処理が必要かもしれないモデルキーに関する「WARNING」、OpenCV カメラが正常に接続されたことを示す「INFO」など、いくつかの情報が含まれています。また、「huggingface/tokenizers: The process current just got forked...」という警告が何度か表示され、フォークによる並列処理の問題を知らせています。この画像は文脈で説明されている Ubuntu の推論コマンドラインに関連し、実行時に現れる可能性のある各種メッセージと警告を示しています。](../../en/images/d51-05.png)
</column>
</grid>

## Mac での推論でアームがカクつくのはなぜか

- データセットが小さすぎる
- GPU のメモリが足りない。50 番台のカードが必要です
