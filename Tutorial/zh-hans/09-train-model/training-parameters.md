[English](../en/09-train-model/training-parameters.md) | 简体中文 | [繁體中文](../zh-hant/09-train-model/training-parameters.md) | [Deutsch](../de/09-train-model/training-parameters.md) | [Español](../es/09-train-model/training-parameters.md) | [Français](../fr/09-train-model/training-parameters.md) | [Italiano](../it/09-train-model/training-parameters.md) | [日本語](../ja/09-train-model/training-parameters.md) | [한국어](../ko/09-train-model/training-parameters.md) | [Português (BR)](../pt-br/09-train-model/training-parameters.md) | [Português (PT)](../pt-pt/09-train-model/training-parameters.md)

# 训练参数建议



https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

可以把`save_freq`参数适当调小，训练后更早能看到模型



对于简单任务（抓取、握手、放置），训练20K个step完全足够