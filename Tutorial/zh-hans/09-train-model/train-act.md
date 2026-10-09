[English](../en/09-train-model/train-act.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# 训练命令行-ACT（推荐入门）

## 参考文档

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## 为什么从ACT算法开始

ACT是玩LeRobot最推荐训练的第一个模型，它的好处如下：

- 模型非常轻量，只有八千万个可学习参数
- 训练收敛速度很快，推理速度也很快
- 在单卡GPU上训练一个小时就能看到效果
- ACT模型下载压缩包大概200MB左右，非常便于存储和传输
- 数据集采集30轮数据基本就够用了
- 可以部署在Ubuntu主机、Mac电脑、Windows电脑，甚至树莓派上推理
- 真实机器人推理效果还很不错，对于夹取、握手、放笔这类简单任务足够了
- LeRobot库的基础环境中已经自带了ACT算法，无需安装其它库

## 命令行

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## 命令行说明

换行符`\`前面只能有一个空格，后面不能有空格

红色为每次运行之前都要检查或者修改的参数

| 命令行参数 | 说明 |
|-|-|
| --dataset.repo_id | HuggingFace数据集的Repo_ID |
| --dataset.root | 数据集本地路径 |
| --dataset.revision | 数据集版本，在上传数据集到HuggingFace的时候指定过的 |
| --dataset.streaming | 数据集在本地，必须为`false`，因为数据集已经在本地，无需流式读取 |
| --dataset.split | 默认为`train`，也就是用全量数据作为训练集 |
| --policy.type | 要训练的算法，比如act、smolvla、diffusion、pi0、wallx |
| --output_dir | 输出结构保存的目录 |
| --job_name | 本次训练任务的名字 |
| --policy.device | 计算设备 |
| --wandb.enable | 开启wandb可视化 |
| --wandb.project | wandb项目名称 |
| --policy.push_to_hub | 将训练好的模型发到HuggingFace云端 |
| --steps | 训练步数 |
| --batch_size | 一步输入的数据量，如果显存不够，应该调小 |
|  |  |

## 训练过程

<grid>
<column width-ratio="0.357753">
![图片展示的是在命令行中使用`lerobot-train`训练命令的示例。命令行中包含多个参数设置，如`--dataset.repo_id`、`--dataset.root`等，用于指定数据集相关信息。还设置了`--policy.type`为`act`，`--output_dir`为`outputs/lerobot_train/output_a`等输出目录，以及`--job_name`、`--policy.device`等其他参数。此外，还列出了`--dataset.split`、`--policy.push_to_hub`等参数的默认值。图片与上下文紧密相关，直观呈现了训练命令行中各参数的设置情况。](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![图片展示的是训练过程中的命令行输出内容。显示了模型训练的相关信息，如scheduler、steps、use_policy_training_preset等参数设置，以及数据集相关参数。还呈现了模型参数数量、损失函数等信息，如num_total_params为55917096（52M），loss为0.626等。下方有文件下载信息，如下载“https://download.pytorch.org/models/resnet18-f37072fd.pth”到/home/featurize/.cache/torch/hub/checkpoints目录。该图片与上下文介绍的训练命令行相关，直观呈现了训练中训练命令行输出的内容。](../../en/images/d36-02.png)
</column>
</grid>

![图片展示的是训练过程中的日志信息。日志以时间顺序记录了训练的多个步骤，包括时间、训练轮数、损失值、学习率等数据。如2024年1月14日15:11:53的训练轮数为131k，损失值为0.368等。其中，`INFO`标识为日志类型，`train`表示训练阶段，`step`为训练轮数，`loss`为损失值，`lr`为学习率。该图片与文档中训练命令行说明部分相关，直观呈现了训练过程中的关键数据。](../../en/images/d36-03.png)

模型压缩包大概300MB