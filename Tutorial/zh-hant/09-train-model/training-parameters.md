[English](../en/09-train-model/training-parameters.md) | [简体中文](../zh-hans/09-train-model/training-parameters.md) | 繁體中文 | [Deutsch](../de/09-train-model/training-parameters.md) | [Español](../es/09-train-model/training-parameters.md) | [Français](../fr/09-train-model/training-parameters.md) | [Italiano](../it/09-train-model/training-parameters.md) | [日本語](../ja/09-train-model/training-parameters.md) | [한국어](../ko/09-train-model/training-parameters.md) | [Português (BR)](../pt-br/09-train-model/training-parameters.md) | [Português (PT)](../pt-pt/09-train-model/training-parameters.md)

# 訓練參數建議



https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

可以把`save_freq`參數適當調小，訓練後更早能看到模型



對於簡單任務（抓取、握手、放置），訓練20K個step完全足夠
