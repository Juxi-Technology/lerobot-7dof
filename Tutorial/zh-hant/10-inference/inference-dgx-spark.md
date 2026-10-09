[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | 繁體中文 | [Deutsch](../de/10-inference/inference-dgx-spark.md) | [Español](../es/10-inference/inference-dgx-spark.md) | [Français](../fr/10-inference/inference-dgx-spark.md) | [Italiano](../it/10-inference/inference-dgx-spark.md) | [日本語](../ja/10-inference/inference-dgx-spark.md) | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# 輝達DGX Spark推論

## 安裝環境

- Pytorch

pytorch單獨從官網安裝cuda13.0版本的

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![圖片展示的是在終端機中安裝Lerobot環境的命令及結果。先是執行了「pip install -e /Downloads/lerobot」命令，接著輸入「python -m lrobot -h」查看Lerobot說明資訊，顯示Lerobot版本為0.4.4。最後執行「pip show lrobot」命令，顯示Lerobot的作者、首頁等資訊。該圖片與文件中安裝Lerobot環境的上下文相關，直觀呈現了安裝過程及結果。](../../en/images/d56-01.png)

- 然後在pyproject.toml檔案裡把torch單獨註解掉

![圖片展示的是在 pyproject.toml 檔案內容，其中用紅色框突出顯示了 torchcode=「2.3.0, c2.8.0」這一行。該檔案是 Python 專案的設定檔，用於指定專案相依性。上下文提到在 pyproject.toml 檔案裡把 torch 單獨註解掉，再 pip install -e，此圖片與上下文相關，直觀呈現了 pyproject.toml 檔案中 torchcode 的位置，為後續操作提供參考。](../../en/images/d56-02.png)

再pip install -e .

- 注意推論命令列裡的policy.path的路徑，換成spark裡面實際的模型路徑

## 刪除原有的eval開頭的資料集（如有）

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

- 安裝環境

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

- 安裝環境

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- 新增程式碼

![圖片展示了lerobot專案中policies資料夾下factory.py檔案的部分程式碼。紅框內關鍵程式碼為「from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors」，以及「processors = make」等語句。該圖片與文件中pi0模型推論部分上下文相關，用於說明在pi0模型推論時，需新增此程式碼以完成相關操作，是pi0模型推論程式碼實現中的重要組成部分。](../../en/images/d56-03.png)

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

- 安裝環境

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
