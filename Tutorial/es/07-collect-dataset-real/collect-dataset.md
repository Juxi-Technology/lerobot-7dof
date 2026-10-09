[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | Español | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# Recopilar un conjunto de datos por demostración

## Eliminar cualquier conjunto de datos existente con el mismo nombre (si lo hay)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## Una cámara, recopilar un conjunto de datos - computadora con Mac

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## Dos cámaras, recopilar un conjunto de datos - computadora con Mac

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=true \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## Durante la recopilación

<grid>
<column width-ratio="0.508765">
![La imagen muestra la interfaz del terminal mientras se recopila un conjunto de datos con OpenVSLAM en un Mac. En la parte superior muestra los parámetros de recopilación, como la resolución, la velocidad de fotogramas y el codificador. Debajo está el registro de recopilación, que registra la hora de inicio de la recopilación, la información de versión, el número de hilos y el codificador, y también muestra el progreso de la recopilación, como 298/298 episodios recopilados, 5119.33 segundos en total. En la parte inferior hay notas para la tecla "ESC", como detener de inmediato y subir el conjunto de datos. Esta imagen se relaciona con el flujo de trabajo de recopilación del conjunto de datos y presenta visualmente la respuesta del terminal durante la recopilación.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![Esta imagen muestra la interfaz del terminal de línea de comandos en un Mac, utilizada para mostrar información del registro de ejecución relacionada con la recopilación del conjunto de datos de las cámaras. Contiene parámetros de configuración relacionados con SVT, como parámetros de config, la versión de la biblioteca de codificación y los valores de cada elemento de configuración (como key frame y CRF, resolución de codificación), y también muestra registros del estado de ejecución, como mensajes sobre el procesamiento de archivos MP4, registros de desconexión de dispositivos y marcas de tiempo mientras se ejecuta el programa. En conjunto, presenta el estado de ejecución en segundo plano durante la recopilación del conjunto de datos de las cámaras.](../../en/images/d26-02.png)
</column>
</grid>

Controles con las teclas de flecha del teclado:  
→ (flecha derecha) Termina el episodio actual antes de tiempo; pasa al siguiente episodio.  
← (flecha izquierda) Cancela el episodio actual; vuelve a grabarlo.  
ESC, detiene de inmediato, codifica el vídeo y sube el conjunto de datos.

## Recopilación finalizada — directorio de guardado del conjunto de datos

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```





## Apretón de manos

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.num_episodes=30 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```
