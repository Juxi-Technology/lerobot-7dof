[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | [Deutsch](../de/09-train-model/train-pi0fast.md) | Español | [Français](../fr/09-train-model/train-pi0fast.md) | [Italiano](../it/09-train-model/train-pi0fast.md) | [日本語](../ja/09-train-model/train-pi0fast.md) | [한국어](../ko/09-train-model/train-pi0fast.md) | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# Línea de comandos de entrenamiento - pi0fast

## Documentación de referencia

https://huggingface.co/docs/lerobot/pi0fast

## Problema

https://github.com/huggingface/lerobot/pull/2203

## Instancia de GPU en la nube recomendada

![Esta imagen muestra los detalles de la instancia de GPU en la nube RTX A6000 recomendada. Indica que hay 3 tarjetas disponibles y que el precio de pago por uso es de 3.29 CNY/hora. La configuración es una GPU RTX A6000 con un total de 51.0 GB de memoria de GPU, una CPU AMD EPYC 7742 de 30 núcleos, 60.9 GB de memoria y 429.5 GB de disco, con un botón "Start Using" abajo. La imagen se sitúa en la sección "Instancia de GPU en la nube recomendada" y ofrece al usuario la recomendación de GPU en la nube, junto con su configuración clave y su precio para el entrenamiento.](../../en/images/d41-01.png)

## Instala el entorno

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## Línea de comandos

- Elimina los archivos de la carpeta output que haya dejado la ejecución de entrenamiento interrumpida anterior

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- Entrena

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi0_fast \
    --output_dir=output_lerobot_train/shake/pi0_fast_A \
    --job_name=shake_pi0_fast_A \
    --policy.pretrained_path=lerobot/pi0_fast_base \
    --policy.dtype=bfloat16 \
    --policy.gradient_checkpointing=true \
    --policy.chunk_size=10 \
    --policy.n_action_steps=10 \
    --policy.max_action_tokens=256 \
    --steps=50000 \
    --batch_size=8 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project
```





## Contenido anterior

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0_fast \
  --output_dir=output_lerobot_train/shake/pi0_fast_A \
  --job_name=shake_pi0_fast_A \
  --policy.pretrained_path=lerobot/pi0_fast_base \
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









Después de ejecutarlo, el entrenamiento solo empieza de verdad pasados unos 10 minutos

El archivo del modelo ocupa unos 5 GB, y unos 7 GB una vez descomprimido
