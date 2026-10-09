[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | [Deutsch](../de/10-inference/infer-smolvla.md) | Español | [Français](../fr/10-inference/infer-smolvla.md) | [Italiano](../it/10-inference/infer-smolvla.md) | [日本語](../ja/10-inference/infer-smolvla.md) | [한국어](../ko/10-inference/infer-smolvla.md) | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# Línea de comandos de inferencia - smolvla

## Ubuntu

- Elimina el conjunto de datos existente con prefijo eval (si lo hay)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Línea de comandos de inferencia

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## Mac

- Elimina el conjunto de datos existente con prefijo eval (si lo hay)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Línea de comandos de inferencia

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/smolvla/40K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=2000
```

<grid>
<column width-ratio="0.425772">
![Esta imagen muestra la interfaz de la línea de comandos de inferencia en un entorno Ubuntu. Arriba muestra el comando que se está ejecutando, incluidos ajustes de parámetros como el uso de la caché y el uso de Delta Joint Actions Aloha. Debajo hay varios datos clave, como el id del robot "zihao_follower_arm", un objetivo relativo máximo de None, el puerto "/dev/tty.usbmodemSAAF2193661" y una nota de que el número de capas del VLM se redujo a 16. La imagen se relaciona con la línea de comandos de inferencia en Ubuntu y presenta la interfaz y algunos de los ajustes de parámetros clave.](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![Esta imagen muestra el terminal durante una sesión de línea de comandos de inferencia en un entorno Ubuntu. Arriba muestra la configuración relacionada con el robot, como calibration_dir y cameras. Debajo aparecen las barras de progreso de carga de varios archivos json, como config.json y processor_config.json, mostrando el porcentaje y el tamaño de carga. En la parte inferior hay mensajes de registro como "Mismatch between calibration values in the motor and the calibration file or no calibration file found", que señala una discrepancia entre los valores de calibración del motor y el archivo de calibración. La imagen corresponde a la línea de comandos de inferencia en Ubuntu y muestra la respuesta del terminal durante la operación.](../../en/images/d50-02.png)
</column>
</grid>

## Resultados

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
