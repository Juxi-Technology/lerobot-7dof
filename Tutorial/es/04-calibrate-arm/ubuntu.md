[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | Español | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Computadora con Ubuntu

## Otorgar permisos al puerto

Dar a todos los usuarios permiso para leer y escribir estos dispositivos serie

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Calibrar el brazo Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![La imagen muestra la interfaz del terminal en una computadora con Ubuntu al ejecutar el comando "lerobot-calibrate" para calibrar el brazo Follower. Muestra la información de conexión del Follower, los nombres de las articulaciones y los valores de límite superior e inferior. La información clave incluye: pulsar Enter para iniciar la calibración, girar cada articulación por sus límites superior e inferior en orden, pulsar Enter para finalizar la calibración; y la ruta del archivo de calibración con "Calibration saved to". Esta imagen está estrechamente relacionada con los pasos para calibrar el brazo Follower y presenta visualmente la respuesta del terminal durante la calibración.](../../en/images/d12-01.png)

## Calibrar el brazo Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![La imagen muestra la interfaz en una computadora con Ubuntu después de otorgar permisos al puerto. En la línea de comandos se introdujo "sudo chmod 666 /dev/ttyACM*" y, tras la ejecución, mostró información como "zihao_leader_arm". Debajo aparecen mensajes como "pulsa Enter para iniciar la calibración", "gira cada articulación por sus límites superior e inferior en orden" y "pulsa Enter para finalizar la calibración", junto con la ruta de calibración "Calibration saved to". Esta imagen corresponde a la sección "Calibrar el brazo Leader" y presenta visualmente la interfaz de preparación antes de la calibración.](../../en/images/d12-02.png)

## Ver el archivo de configuración de la calibración

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![La imagen muestra el contenido del archivo "zihao_follower_arm.json" mostrado en el terminal de Ubuntu. El archivo contiene información de configuración de varias articulaciones, como shoulder_pan, shoulder_lift, elbow_flex y wrist_flex, y cada articulación tiene parámetros como id, drive_mode, homing_offset, range_min y range_max. Esta imagen se relaciona con la sección "Ver el archivo de configuración de la calibración" y presenta visualmente la información específica de los parámetros del archivo de calibración, lo que ayuda al usuario a comprender la configuración de cada articulación.](../../en/images/d12-03.png)



## Notas

### ① Un brazo deja de moverse al alcanzar un límite

Hay que recalibrarlo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② No se encuentran los servos

![Esta es una captura de pantalla que muestra una interfaz de error en el terminal de Ubuntu, correspondiente a la nota "no se encuentran los servos". La interfaz informa un RuntimeError, concretamente "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'", es decir, falló la comprobación de los servos. También enumera la información esperada de los servos, con IDs de motor esperados del 1 al 6 y modelo esperado 777, pero la lista de motores realmente encontrados está vacía; junto con el contexto, este error se debe a que los servos no están alimentados.](../../en/images/d12-04.png)

La alimentación de los servos no está conectada
