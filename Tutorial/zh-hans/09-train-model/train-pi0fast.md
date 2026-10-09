[English](../en/09-train-model/train-pi0fast.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | [Deutsch](../de/09-train-model/train-pi0fast.md) | [Español](../es/09-train-model/train-pi0fast.md) | [Français](../fr/09-train-model/train-pi0fast.md) | [Italiano](../it/09-train-model/train-pi0fast.md) | [日本語](../ja/09-train-model/train-pi0fast.md) | [한국어](../ko/09-train-model/train-pi0fast.md) | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# 训练命令行-pi0fast

## 参考文档

https://huggingface.co/docs/lerobot/pi0fast

## Issue

https://github.com/huggingface/lerobot/pull/2203

## 推荐云GPU实例

![图片展示了推荐的云GPU实例RTX A6000的相关信息。显示有3卡可用，按量使用价格为每小时3.29元。其配置为GPU RTX A6000，显存共51.0GB，CPU是30核AMD EPYC 7742，内存60.9GB，硬盘429.5GB，底部还有“开始使用”按钮。该图片位于文档“推荐云GPU实例”部分，为用户提供训练所需的云GPU实例推荐及关键配置和价格等信息。](../../en/images/d41-01.png)

## 安装环境

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## 命令行

- 删除之前训练中断的output下的文件

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- 训练

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi0_fast \
    --output_dir=output_lerobot_train/shake/pi0_fast_A \
    --job_name=shake_pi0_fast_A \
    --policy.pretrained_path=lerobot/pi0_fast_base \
    --policy.dtype=bfloat16 \
    --policy.gradient_checkpointing=true \
    --policy.chunk_size=10 \
    --policy.n_action_steps=10 \
    --policy.max_action_tokens=256 \
    --steps=50000 \
    --batch_size=8 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project
```





## 之前的内容

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0_fast \
  --output_dir=output_lerobot_train/shake/pi0_fast_A \
  --job_name=shake_pi0_fast_A \
  --policy.pretrained_path=lerobot/pi0_fast_base \
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









运行后10分钟左右，训练才会正式开始

模型压缩包5个G左右，解压后7个G