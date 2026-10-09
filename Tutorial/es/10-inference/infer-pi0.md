[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | Español | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# Línea de comandos de inferencia - pi0

## Ubuntu

- Elimina el conjunto de datos existente con prefijo eval (si lo hay)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
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
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![Esta imagen muestra un error que aparece al conectarse a la máquina por SSH en un entorno Ubuntu. Muestra un error de consola que indica que la plataforma no es compatible, que no se puede establecer la conexión X y que conviene asegurarse de que hay un servidor X en ejecución y de que la variable de entorno DISPLAY está configurada correctamente. También muestra una advertencia sobre un entorno sin interfaz gráfica (headless) y un registro de que se grabó el episodio 0. La imagen se relaciona con la línea de comandos de inferencia en Ubuntu y puede tratarse de una situación anómala encontrada durante la operación.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![Esta imagen muestra la salida al ejecutar la línea de comandos de inferencia en un entorno Ubuntu. Durante la ejecución aparecen varias veces mensajes de error "E0119", que indican que no hay una configuración válida de triton durante el autotuning y que los recursos están agotados, como memoria compartida insuficiente. También muestra los parámetros en tiempo de ejecución de varios modelos triton_mm, como ALLOW_TF32, BLOCK_K y BLOCK_M, junto con los valores correspondientes de ACC_TYPE, ALLOW_TF32, BLOCK_K y BLOCK_M. La imagen se relaciona con la línea de comandos de inferencia en Ubuntu y muestra una escasez de recursos encontrada durante la ejecución.](../../en/images/d51-02.png)
</column>
</grid>

![Esta imagen muestra el terminal durante una sesión de línea de comandos de inferencia en un entorno Ubuntu. Muestra los resultados de varias instrucciones triton_mm; por ejemplo, triton_mm_3644 tardando 0.2355 ms, todas usando el tipo t1.float32 con ALLOW_TF32=True, y también muestra parámetros como BLOCK_K. Al final muestra SingleProcess AUTOTUNE benchmarking tardando 0.7305 segundos y 0.0001 segundos en precompilar 20 opciones. La imagen se relaciona con la línea de comandos de inferencia en Ubuntu y muestra la ejecución real.](../../en/images/d51-03.png)

> **Vídeo pendiente**: el texto original inserta aquí `VID_20260120_182109.mp4` (originalmente 310 MB). Del lado de Feishu no se proporcionó ningún flujo de vídeo descargable para este archivo, solo metadatos, por lo que no pudo capturarse. Para verlo, consulta el [documento original](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg).



## Mac

- Elimina el conjunto de datos existente con prefijo eval (si lo hay)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Línea de comandos de inferencia

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![Esta imagen muestra el terminal durante una sesión de línea de comandos de inferencia (11 - yolo26) en un entorno Ubuntu. Muestra la información de versión de Python 3.12 y un registro de robot-type ajustado a follower. También lista parámetros relacionados con la cámara, como color_mode, fourcc, fps, height y width, y muestra la ruta desde la que se carga el modelo junto con algunos mensajes de advertencia, como errores de carga del modelo. La imagen se relaciona con la línea de comandos de inferencia en Ubuntu y presenta la respuesta del terminal durante la operación.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![Esta imagen muestra la salida de la línea de comandos al ejecutar la inferencia con código Python en un entorno Ubuntu. Contiene varios elementos de información, como "PIBPytorch model" cargándose correctamente, "WARNING" sobre claves del modelo que quizá haya que gestionar, e "INFO" indicando que la cámara de OpenCV se conectó correctamente. También muestra varias veces la advertencia "huggingface/tokenizers: The process current just got forked...", que señala un problema de paralelismo causado por el fork. La imagen se relaciona con la línea de comandos de inferencia en Ubuntu descrita en el contexto y muestra los distintos mensajes y advertencias que pueden aparecer en tiempo de ejecución.](../../en/images/d51-05.png)
</column>
</grid>

## Por qué la inferencia en un Mac hace que el brazo tiemble

- El conjunto de datos es demasiado pequeño
- La GPU no tiene memoria suficiente; necesitas una tarjeta de la serie 50
