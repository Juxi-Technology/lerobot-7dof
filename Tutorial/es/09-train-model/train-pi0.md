[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | [Deutsch](../de/09-train-model/train-pi0.md) | Español | [Français](../fr/09-train-model/train-pi0.md) | [Italiano](../it/09-train-model/train-pi0.md) | [日本語](../ja/09-train-model/train-pi0.md) | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# Línea de comandos de entrenamiento - pi0 (mejores resultados)

## Documentación de referencia

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## Instancia de GPU en la nube recomendada

![Esta imagen muestra los detalles de una instancia de GPU en la nube RTX A6000. Su precio de pago por uso es de 3.29 CNY/hora; la GPU es una RTX A6000 con un total de 51.0 GB de memoria de GPU; la CPU es un AMD EPYC 7742 de 30 núcleos; la memoria es de 60.9 GB; y el disco, de 429.5 GB. En la esquina superior derecha se indica que hay 3 tarjetas disponibles. Abajo hay un botón azul «Start Using». La imagen se relaciona con la sección «Instancia de GPU en la nube recomendada» y presenta visualmente la configuración, el precio y demás información clave de la GPU en la nube recomendada.](../../en/images/d39-01.png)

## Instala el entorno

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## Línea de comandos

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_A

lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0 \
  --output_dir=~/output_lerobot_train/shake/pi0_A \
  --job_name=shake_pi0_A \
  --policy.pretrained_path=lerobot/pi0_base \
  --policy.compile_model=true \
  --policy.gradient_checkpointing=true \
  --policy.dtype=bfloat16 \
  --policy.freeze_vision_encoder=false \
  --policy.train_expert_only=false \
  --steps=50000 \
  --policy.device=cuda \
  --policy.push_to_hub=false \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --batch_size=8
```

Después de que la línea de comandos lleve 20 minutos ejecutándose, el entrenamiento solo entonces empieza de verdad

El archivo del modelo ocupa unos 5 GB, y unos 7 GB una vez descomprimido
