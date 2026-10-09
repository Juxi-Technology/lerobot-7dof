[English](../en/09-train-model/get-model-weights.md) | [简体中文](../zh-hans/09-train-model/get-model-weights.md) | [繁體中文](../zh-hant/09-train-model/get-model-weights.md) | [Deutsch](../de/09-train-model/get-model-weights.md) | [Español](../es/09-train-model/get-model-weights.md) | [Français](../fr/09-train-model/get-model-weights.md) | [Italiano](../it/09-train-model/get-model-weights.md) | [日本語](../ja/09-train-model/get-model-weights.md) | [한국어](../ko/09-train-model/get-model-weights.md) | Português (BR) | [Português (PT)](../pt-pt/09-train-model/get-model-weights.md)

# Obtenção dos arquivos de peso do modelo



```Shell
cd ~/output_lerobot_train/a/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```

Os pesos do modelo são salvos uma vez a cada 20K passos.



Clique com o botão direito para baixar o arquivo `ckpt.zip` e extraí-lo no seu computador local


















## Aperto de mão

```Shell
# Um modelo salvo em um passo de treinamento específico
# cd ~/output_lerobot_train/shake/diffusion/checkpoints/005000

# O modelo mais recente
cd ~/output_lerobot_train/shake/wallx/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```
