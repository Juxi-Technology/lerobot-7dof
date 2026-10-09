[English](../en/09-train-model/train-pi0.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | [Deutsch](../de/09-train-model/train-pi0.md) | [Español](../es/09-train-model/train-pi0.md) | [Français](../fr/09-train-model/train-pi0.md) | [Italiano](../it/09-train-model/train-pi0.md) | [日本語](../ja/09-train-model/train-pi0.md) | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# 训练命令行-pi0（效果最好）

## 参考文档

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## 推荐云GPU实例

![图片展示的是RTX A6000云GPU实例信息。其按量使用价格为3.29元/小时，GPU为RTX A6000，共51.0GB显存；CPU为30核AMD EPYC 7742；内存60.9GB；硬盘429.5GB。右上角显示有3卡可用。底部有一个蓝色的“开始使用”按钮。该图片与文档中“推荐云GPU实例”部分内容相关，直观呈现了推荐的云GPU实例配置及价格等关键信息。](../../en/images/d39-01.png)

## 安装环境

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## 命令行

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_A

lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0 \
  --output_dir=~/output_lerobot_train/shake/pi0_A \
  --job_name=shake_pi0_A \
  --policy.pretrained_path=lerobot/pi0_base \
  --policy.compile_model=true \
  --policy.gradient_checkpointing=true \
  --policy.dtype=bfloat16 \
  --policy.freeze_vision_encoder=false \
  --policy.train_expert_only=false \
  --steps=50000 \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --batch_size=8
```

命令行运行20分钟后，训练才会正式开始

模型压缩包5个G左右，解压后7个G