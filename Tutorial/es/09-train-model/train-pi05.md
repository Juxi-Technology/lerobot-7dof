[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | Español | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# Línea de comandos de entrenamiento - pi0.5

## Documentación de referencia

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## Instancia de GPU en la nube recomendada

![Esta imagen muestra los detalles de una instancia de GPU en la nube RTX A6000 ofrecida por Alibaba Cloud. Su precio de pago por uso es de 3.29 CNY/hora y hay 3 tarjetas disponibles. La instancia está configurada con una GPU RTX A6000 con un total de 51.0 GB de memoria de GPU, una CPU AMD EPYC 7742 de 30 núcleos, 60.9 GB de memoria y 429.5 GB de disco. Abajo hay un botón azul «Start Using». La imagen se relaciona con la sección «Instancia de GPU en la nube recomendada» y presenta visualmente la configuración, el precio y demás información clave de la GPU en la nube recomendada.](../../en/images/d40-01.png)

## Instala el entorno

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## Línea de comandos

- Elimina los archivos de la carpeta output que haya dejado la ejecución de entrenamiento interrumpida anterior

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- Entrena

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi05 \
    --output_dir=~/output_lerobot_train/shake/pi05_A \
    --job_name=shake_pi05_A \
    --policy.pretrained_path=lerobot/pi05_base \
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

<grid>
<column width-ratio="0.468128">
![Esta imagen muestra la salida al entrenar un modelo de inteligencia física (PI) desde la línea de comandos. Muestra el modelo cargándose, parámetros reasignándose y el optimizador y el planificador creándose; por ejemplo, "Loading model from: lerobot/pi05_base". También aparecen mensajes de advertencia como "Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy". Presenta además cifras relacionadas con el entrenamiento como "num_total_frames: 180K". La imagen se relaciona con la operación de la línea de comandos de entrenamiento y presenta visualmente la información clave de la ejecución.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![Esta imagen muestra la salida durante el entrenamiento desde la línea de comandos. Durante el entrenamiento se bifurca (fork) el proceso de huggingface/torch; como el paralelismo ya está en uso, este se desactiva para evitar un interbloqueo, con un mensaje que aconseja evitar hacerlo "before the fork if possible". El entrenamiento solo empieza de verdad pasados 20 minutos. La imagen guarda una relación estrecha con el contexto y presenta las operaciones de proceso y los mensajes de aviso que pueden aparecer durante el entrenamiento, ayudando a explicar el estado y los puntos a los que conviene prestar atención.](../../en/images/d40-02.png)
</column>
</grid>

Después de que la línea de comandos lleve 20 minutos ejecutándose, el entrenamiento solo entonces empieza de verdad

El archivo del modelo ocupa unos 5 GB, y unos 7 GB una vez descomprimido
