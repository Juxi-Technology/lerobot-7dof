[English](../en/09-train-model/get-model-weights.md) | [简体中文](../zh-hans/09-train-model/get-model-weights.md) | [繁體中文](../zh-hant/09-train-model/get-model-weights.md) | [Deutsch](../de/09-train-model/get-model-weights.md) | [Español](../es/09-train-model/get-model-weights.md) | [Français](../fr/09-train-model/get-model-weights.md) | Italiano | [日本語](../ja/09-train-model/get-model-weights.md) | [한국어](../ko/09-train-model/get-model-weights.md) | [Português (BR)](../pt-br/09-train-model/get-model-weights.md) | [Português (PT)](../pt-pt/09-train-model/get-model-weights.md)

# Ottenere i file dei pesi del modello



```Shell
cd ~/output_lerobot_train/a/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```

I pesi del modello vengono salvati una volta ogni 20K step.



Fai clic con il pulsante destro per scaricare l'archivio `ckpt.zip` ed estrarlo sul tuo computer locale
















## Shake Hands

```Shell
# Un modello salvato a uno step di addestramento specifico
# cd ~/output_lerobot_train/shake/diffusion/checkpoints/005000

# Il modello più recente
cd ~/output_lerobot_train/shake/wallx/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```
