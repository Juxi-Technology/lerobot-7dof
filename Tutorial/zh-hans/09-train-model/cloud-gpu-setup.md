[English](../en/09-train-model/cloud-gpu-setup.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# 云GPU训练环境配置

## 关闭自己电脑的网络代理

不然可能打不开Jupyter的命令行

## 登录云GPU平台Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

加用户群，跟客服说是同济子豪兄粉丝，领取代金券

## 开启一个云GPU实例

## 安装配置环境

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## 登录wandb

```Shell
wandb login
复制粘贴API Key，回车
```

![图片展示了在Lerobot环境下使用`wandb login`命令登录W&B（Weights & Biases）的过程。命令执行后，W&B提示登录到https://api.wandb.ai，可部署本地W&B服务器，并引导查找API密钥。接着，W&B提示将API密钥粘贴并回车，还指出未找到.netrc文件，正在创建。最后，W&B显示已登录为tommyzihao，登录状态为https://api.wandb.ai。该图片与文档中“登录wandb”步骤相关，直观呈现了登录操作的反馈信息。](../../en/images/d35-01.png)

## 挂载数据集

```Shell
复制实例下载命令，类似：
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

数据集出现在`~`目录下

## 修改权重保存频率（选做）

打开`lerobot/src/lerobot/configs/train.py`

将save_freq，从20_000修改为5_000

这样能在训练更早期获得模型权重文件
