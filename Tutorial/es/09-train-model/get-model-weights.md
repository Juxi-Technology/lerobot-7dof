[English](../en/09-train-model/get-model-weights.md) | [简体中文](../zh-hans/09-train-model/get-model-weights.md) | [繁體中文](../zh-hant/09-train-model/get-model-weights.md) | [Deutsch](../de/09-train-model/get-model-weights.md) | Español | [Français](../fr/09-train-model/get-model-weights.md) | [Italiano](../it/09-train-model/get-model-weights.md) | [日本語](../ja/09-train-model/get-model-weights.md) | [한국어](../ko/09-train-model/get-model-weights.md) | [Português (BR)](../pt-br/09-train-model/get-model-weights.md) | [Português (PT)](../pt-pt/09-train-model/get-model-weights.md)

# Obtener los archivos de pesos del modelo



```Shell
cd ~/output_lerobot_train/a/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```

Los pesos del modelo se guardan una vez cada 20K pasos.



Haz clic derecho para descargar el archivo `ckpt.zip` y descomprimirlo en tu ordenador local




















## Dar la mano

```Shell
# A model saved at a specific training step
# cd ~/output_lerobot_train/shake/diffusion/checkpoints/005000

# The latest model
cd ~/output_lerobot_train/shake/wallx/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```
