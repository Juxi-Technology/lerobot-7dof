[English](../en/09-train-model/get-model-weights.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/get-model-weights.md) | [Deutsch](../de/09-train-model/get-model-weights.md) | [Español](../es/09-train-model/get-model-weights.md) | [Français](../fr/09-train-model/get-model-weights.md) | [Italiano](../it/09-train-model/get-model-weights.md) | [日本語](../ja/09-train-model/get-model-weights.md) | [한국어](../ko/09-train-model/get-model-weights.md) | [Português (BR)](../pt-br/09-train-model/get-model-weights.md) | [Português (PT)](../pt-pt/09-train-model/get-model-weights.md)

# 获得模型权重文件



```Shell
cd ~/output_lerobot_train/a/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```

每20K个step，保存一次模型权重文件



右键下载`ckpt.zip`压缩包，解压到本地电脑















## 握手

```Shell
# 指定训练步数保存的模型
# cd ~/output_lerobot_train/shake/diffusion/checkpoints/005000

# 最新的模型
cd ~/output_lerobot_train/shake/wallx/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```