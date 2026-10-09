[English](../en/10-inference/inference-dgx-spark.md) | 简体中文 | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | [Deutsch](../de/10-inference/inference-dgx-spark.md) | [Español](../es/10-inference/inference-dgx-spark.md) | [Français](../fr/10-inference/inference-dgx-spark.md) | [Italiano](../it/10-inference/inference-dgx-spark.md) | [日本語](../ja/10-inference/inference-dgx-spark.md) | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# 英伟达DGX Spark推理

## 安装环境

- Pytorch

pytorch单独从官网安装cuda13.0版本的

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![图片展示的是在终端中安装Lerobot环境的命令及结果。先是执行了“pip install -e /Downloads/lerobot”命令，接着输入“python -m lrobot -h”查看Lerobot帮助信息，显示Lerobot版本为0.4.4。最后执行“pip show lrobot”命令，显示Lerobot的作者、主页等信息。该图片与文档中安装Lerobot环境的上下文相关，直观呈现了安装过程及结果。](../../en/images/d56-01.png)

- 然后在pyproject.toml文件里把torch单独注释掉

![图片展示的是在 pyproject.toml 文件内容，其中用红色框突出显示了 torchcode=“2.3.0, c2.8.0”这一行。该文件是 Python 项目的配置文件，用于指定项目依赖。上下文提到在 pyproject.toml 文件里把 torch 单独注释掉，再 pip install -e，此图片与上下文相关，直观呈现了 pyproject.toml 文件中 torchcode 的位置，为后续操作提供参考。](../../en/images/d56-02.png)

再pip install -e .

- 注意推理命令行里的policy.path的路径，换成spark里面实际的模型路径

## 删除原有的eval开头的数据集（如有）

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

- 安装环境

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- 推理

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

- 安装环境

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- 添加代码

![图片展示了lerobot项目中policies文件夹下factory.py文件的部分代码。红框内关键代码为“from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors”，以及“processors = make”等语句。该图片与文档中pi0模型推理部分上下文相关，用于说明在pi0模型推理时，需添加此代码以完成相关操作，是pi0模型推理代码实现中的重要组成部分。](../../en/images/d56-03.png)

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

- 安装环境

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- 推理

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