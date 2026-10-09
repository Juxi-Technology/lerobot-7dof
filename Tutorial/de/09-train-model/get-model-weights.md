[English](../en/09-train-model/get-model-weights.md) | [简体中文](../zh-hans/09-train-model/get-model-weights.md) | [繁體中文](../zh-hant/09-train-model/get-model-weights.md) | Deutsch | [Español](../es/09-train-model/get-model-weights.md) | [Français](../fr/09-train-model/get-model-weights.md) | [Italiano](../it/09-train-model/get-model-weights.md) | [日本語](../ja/09-train-model/get-model-weights.md) | [한국어](../ko/09-train-model/get-model-weights.md) | [Português (BR)](../pt-br/09-train-model/get-model-weights.md) | [Português (PT)](../pt-pt/09-train-model/get-model-weights.md)

# Modellgewicht-Dateien abrufen



```Shell
cd ~/output_lerobot_train/a/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```

Die Modellgewichte werden alle 20K Schritte gespeichert.



Klicken Sie mit der rechten Maustaste, um das Archiv `ckpt.zip` herunterzuladen, und entpacken Sie es auf Ihren lokalen Computer



















## Shake Hands

```Shell
# Ein bei einem bestimmten Trainingsschritt gespeichertes Modell
# cd ~/output_lerobot_train/shake/diffusion/checkpoints/005000

# Das neueste Modell
cd ~/output_lerobot_train/shake/wallx/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```
