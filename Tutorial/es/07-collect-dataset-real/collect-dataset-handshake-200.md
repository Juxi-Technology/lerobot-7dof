[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | Español | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# Recopilar un conjunto de datos por demostración — Handshake 200

## Crear un repositorio de conjunto de datos en HuggingFace

https://huggingface.co/new-dataset

![La imagen muestra la interfaz de creación de un nuevo repositorio de conjunto de datos en HuggingFace. En "Owner" se muestra TommyZihao, en el campo "Dataset name" se ha escrito lerobot_zihao_dataset_shake200, en "License" se ha seleccionado mit y la opción "Public" está marcada; debajo se indica que cualquier persona puede ver este conjunto de datos y que solo el propietario del conjunto de datos o los miembros de la organización pueden hacer commits. En la parte inferior se avisa de que, una vez creado el conjunto de datos, se pueden subir archivos mediante la interfaz web o git, y hay un botón "Create dataset". Esta imagen se relaciona con el contenido sobre crear un repositorio de conjunto de datos en HuggingFace y muestra la interfaz de la operación de creación.](../../en/images/d27-01.png)

## Eliminar cualquier conjunto de datos existente con el mismo nombre (si lo hay)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Recopilación del conjunto de datos Shake200

Una cámara, recopilar un conjunto de datos - computadora con Mac

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
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake200 \
    --dataset.num_episodes=200 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```

## Durante la recopilación

<grid>
<column width-ratio="0.508765">
![Este es el contenido de la interfaz del terminal durante la grabación de pantalla al recopilar el conjunto de datos de apretón de manos (Shake200). Tanto en la parte superior como en la parte central hacia abajo de la interfaz aparece el mensaje "INFO 202-04-15 17:00:22.227 Recording episode 36", que indica que se está grabando el episodio de recopilación número 36; también se muestran parámetros relacionados con el codificador SVT-AV1 y la información de progreso "200/200 \[00:00:00::00, 4911.09 examples/s\]", lo que refleja que en esta fase se está llevando a cabo la recopilación del conjunto de datos, corresponde al apartado "Durante la recopilación" descrito en el documento y muestra el estado de funcionamiento del sistema durante la recopilación.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![Esta imagen es la interfaz de ejecución del terminal de Mac y muestra la configuración del proyecto SVT y la información del registro de ejecución. La interfaz incluye varios parámetros de configuración, como los valores del tamaño de GOP, el tamaño de mini-pod, el tipo de fotograma clave y el CRF de la configuración de SVT, además de la información de versión de la biblioteca SVT-A1 Encoder. El registro de la mitad inferior de la interfaz recoge contenido relacionado con operaciones sobre archivos de vídeo: menciona el traslado del átomo al principio del archivo y el estado de conexión y desconexión de componentes como varias OpenVCamera y SV280 follower, y por último muestra el aviso de fin de ejecución del terminal en la ruta actual. Corresponde al escenario de ejecución del terminal durante el proceso de recopilación del conjunto de datos Handshake 200.](../../en/images/d27-02.png)
</column>
</grid>

Controles con las teclas de flecha del teclado:  
→ (flecha derecha) Termina el episodio actual antes de tiempo; pasa al siguiente episodio.  
← (flecha izquierda) Cancela el episodio actual; vuelve a grabarlo.  
ESC, detiene de inmediato, codifica el vídeo y sube el conjunto de datos.

## Recopilación finalizada — directorio de guardado del conjunto de datos

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
