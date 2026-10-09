[English](../en/09-train-model/training-parameters.md) | [简体中文](../zh-hans/09-train-model/training-parameters.md) | [繁體中文](../zh-hant/09-train-model/training-parameters.md) | [Deutsch](../de/09-train-model/training-parameters.md) | [Español](../es/09-train-model/training-parameters.md) | Français | [Italiano](../it/09-train-model/training-parameters.md) | [日本語](../ja/09-train-model/training-parameters.md) | [한국어](../ko/09-train-model/training-parameters.md) | [Português (BR)](../pt-br/09-train-model/training-parameters.md) | [Português (PT)](../pt-pt/09-train-model/training-parameters.md)

# Recommandations sur les paramètres d'entraînement



https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

Vous pouvez réduire un peu le paramètre `save_freq` afin que le modèle apparaisse plus tôt après l'entraînement



Pour les tâches simples (saisir, serrer la main, poser), 20K steps d'entraînement suffisent amplement
