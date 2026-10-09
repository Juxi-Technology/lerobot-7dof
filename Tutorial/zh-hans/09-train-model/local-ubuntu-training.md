[English](../en/09-train-model/local-ubuntu-training.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# 本地Ubuntu训练

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- 注意

`\`前面只能有一个空格，后面不能有空格

`--dataset.split`默认为`train`，也就是用全量数据作为训练集

数据集在本地，`--dataset.streaming`必须为`false`，因为数据集已经在本地，无需流式读取

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![图片展示了在Ubuntu本地环境下使用lerobot_train.py脚本进行训练的命令及配置信息。命令中包含数据集路径、仓库ID、分支等参数，如`--dataset.repo_id`为Tommy/lerobot_zhao_dataset_a等。配置信息中，`--dataset.streaming`设为`false`，`--use_imagenet_stats`为`True`，`--batch_size`为4，`--val_n_episodes`为1000等。图片与上下文紧密相关，直观呈现了训练时的命令及关键配置参数。](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![这张图片展示了在本地Ubuntu环境下运行lerobot_train.py脚本时的训练日志内容，核心是训练相关的配置参数与训练过程的实时状态信息。图片中明确标记了训练的关键信息，包括`--dataset.split`默认值为`train`，即采用全量数据作为训练集，同时因数据集存于本地，对应`--dataset.streaming`的状态明确。日志内容还包含了训练的进度信息、数据集加载情况、模型优化器与调度器的创建信息，以及训练过程中的损耗、步数相关的数值数据，直观呈现了本地训练进行中的状态。](../../en/images/d34-02.png)
</column>
</grid>

![图片展示的是在Ubuntu环境下使用LeroBot训练模型时的训练日志。日志中记录了训练过程中的信息，如时间、训练集、模型、损失值、准确率等。其中，时间显示为2024年1月14日15:11:53至16:15:16，损失值（loss）在0.68 - 0.65之间波动，准确率（acc）在0.85 - 0.88之间。该图片与文档中LeroBot训练模型的上下文相关，直观呈现了训练过程中的关键指标变化情况。](../../en/images/d34-03.png)