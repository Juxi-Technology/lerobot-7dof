[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | Español | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# Entrenamiento local en Ubuntu

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- Nota

`\` solo puede tener un espacio delante y ninguno detrás

`--dataset.split` tiene el valor predeterminado `train`, lo que significa que se usa el conjunto de datos completo como conjunto de entrenamiento

El conjunto de datos es local, por lo que `--dataset.streaming` debe ser `false`, ya que los datos ya están en disco y no hace falta una lectura en streaming

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![Esta imagen muestra el comando de entrenamiento y la configuración para ejecutar el script lerobot_train.py en un entorno Ubuntu local. El comando incluye parámetros como la ruta del conjunto de datos, el ID del repositorio y la rama; por ejemplo, `--dataset.repo_id` ajustado a Tommy/lerobot_zhao_dataset_a. Entre los valores de configuración, `--dataset.streaming` está en `false`, `--use_imagenet_stats` en `True`, `--batch_size` en 4 y `--val_n_episodes` en 1000. La imagen guarda una relación estrecha con el contexto y presenta visualmente el comando de entrenamiento y sus parámetros de configuración clave.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![Esta imagen muestra el registro de entrenamiento generado al ejecutar el script lerobot_train.py en un entorno Ubuntu local, centrado en los parámetros de configuración del entrenamiento y en el estado en vivo de la ejecución. Marca con claridad información clave: `--dataset.split` tiene el valor predeterminado `train`, lo que significa que se usa el conjunto de datos completo como conjunto de entrenamiento, y, dado que el conjunto de datos está almacenado localmente, el estado de `--dataset.streaming` también queda fijado de la misma manera. El registro abarca además el progreso del entrenamiento, la carga del conjunto de datos, la creación del optimizador y el planificador del modelo, y las cifras de pérdida y de paso durante el entrenamiento, ofreciendo una visión clara de una ejecución de entrenamiento local en curso.](../../en/images/d34-02.png)
</column>
</grid>

![Esta imagen muestra el registro de entrenamiento generado al entrenar un modelo con LeRobot en un entorno Ubuntu. El registro recoge información de la ejecución, como la hora, el conjunto de entrenamiento, el modelo, la pérdida y la precisión. Las marcas de tiempo van de 15:11:53 a 16:15:16 del 14 de enero de 2024, la pérdida fluctúa entre 0.68 y 0.65, y la precisión (acc) entre 0.85 y 0.88. La imagen se relaciona con el contexto de entrenamiento del modelo LeRobot y presenta visualmente cómo cambian las métricas clave durante el entrenamiento.](../../en/images/d34-03.png)
