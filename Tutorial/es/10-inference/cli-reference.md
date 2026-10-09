[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | Español | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# Referencia de la línea de comandos

## Notas sobre la línea de comandos

Con visualización en vivo: --display_data=true

Sin visualización en vivo: --display_data=false

Con `--display_data=true`, se lanza la genial interfaz de visualización rerun.io, pero en el directorio `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` se guarda una imagen por cada fotograma, lo que ocupa mucho espacio. Puedes ponerlo en `--display_data=false` más adelante.



Inferir un modelo desde un repositorio de modelos de HuggingFace: --policy.path=Tommymy/lerobot_my_model_a



## Usando la tarea de recoger naranjas como ejemplo

- Infiere un modelo local (con visualización en vivo)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Infiere un modelo local (sin visualización en vivo)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Infiere un modelo desde un repositorio de modelos de HuggingFace

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --policy.path=Tommymy/lerobot_my_model_a
```

El modelo se descarga tras ejecutarse

![Esta imagen muestra la interfaz para ejecutar el script `pretrained_model.py` en la línea de comandos. Arriba muestra la configuración de parámetros del modelo, como `--display_data=true` y `--policy.path=TommyZihao/lerobot_zihao_model_a`. Debajo aparecen ajustes de parámetros como "robot", "camera" y "calibration_dir". En la parte inferior se muestra el progreso de descarga del modelo, actualmente al 68%. La imagen se relaciona con la descripción de la ejecución del script `pretrained_model.py` y sus parámetros, y presenta visualmente los ajustes de parámetros y el progreso de descarga.](../../en/images/d48-01.png)
