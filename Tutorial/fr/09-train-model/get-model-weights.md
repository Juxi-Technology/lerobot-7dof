[English](../en/09-train-model/get-model-weights.md) | [简体中文](../zh-hans/09-train-model/get-model-weights.md) | [繁體中文](../zh-hant/09-train-model/get-model-weights.md) | [Deutsch](../de/09-train-model/get-model-weights.md) | [Español](../es/09-train-model/get-model-weights.md) | Français | [Italiano](../it/09-train-model/get-model-weights.md) | [日本語](../ja/09-train-model/get-model-weights.md) | [한국어](../ko/09-train-model/get-model-weights.md) | [Português (BR)](../pt-br/09-train-model/get-model-weights.md) | [Português (PT)](../pt-pt/09-train-model/get-model-weights.md)

# Obtenir les fichiers de poids du modèle



```Shell
cd ~/output_lerobot_train/a/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```

Les poids du modèle sont enregistrés une fois tous les 20K steps.



Cliquez avec le bouton droit pour télécharger l'archive `ckpt.zip` et la décompresser sur votre ordinateur local
















## Poignée de main

```Shell
# Un modèle enregistré à une étape d'entraînement précise
# cd ~/output_lerobot_train/shake/diffusion/checkpoints/005000

# Le modèle le plus récent
cd ~/output_lerobot_train/shake/wallx/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```
