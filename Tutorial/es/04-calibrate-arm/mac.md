[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | Español | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Computadora con Mac

## Revisar los números de puerto

Brazo Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Brazo Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Calibrar el brazo Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![La imagen muestra la interfaz de línea de comandos para calibrar los servos del SO101 en un Mac. El comando es "lerobot-calibrate", con parámetros como robot.type, robot.port y robot.id. La interfaz muestra información de configuración del robot, como "zihao_follower_arm". Debajo se te pide que pulses "c" y Enter para iniciar la calibración, y también muestra mensajes como "zihao_follower_arm SO101Follower connected". Esta imagen corresponde a la sección "Calibrar el brazo Follower" y presenta visualmente el comando de calibración y la respuesta de la interfaz.](../../en/images/d13-01.png)

![La imagen muestra la interfaz de línea de comandos de una operación de calibración de LeRobot en Ubuntu. El comando es "lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm" y muestra la información de calibración del Follower, incluidas la posición mínima, máxima y actual de cada articulación. Los mensajes clave de la operación están resaltados con recuadros rojos, como "pulsa Enter para iniciar la calibración", "gira cada articulación por sus límites superior e inferior en orden" y "pulsa Enter para finalizar la calibración", en consonancia con los pasos de calibración descritos en el contexto.](../../en/images/d13-02.png)

## Calibrar el brazo Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![La imagen muestra la interfaz de línea de comandos de una calibración de LeRobot en Ubuntu. La línea de comandos ejecutó operaciones como "sudo chmod 666 /dev/ttyACM*" y "lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1", mostrando la información de los números de puerto de los brazos Follower y Leader. La interfaz también te pide que pulses Enter para iniciar la calibración, que gires cada articulación por sus límites superior e inferior en orden y que pulses Enter para finalizar, y por último muestra la ruta donde se guarda el archivo de configuración de la calibración. Esta imagen se relaciona con el contenido de calibración de LeRobot y presenta visualmente los pasos de calibración.](../../en/images/d13-03.png)

## Ver el archivo de configuración de la calibración

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## Errores comunes

- No se encuentran uno o varios de los servos

![La imagen muestra la información de parámetros de los servos mostrada durante la calibración del Follower SO. En la parte superior se muestra la información de conexión y un mensaje de calibración que te pide mover el Follower al centro de su rango de movimiento y pulsar ENTER, luego recorrer en orden el rango de movimiento de todas las articulaciones, registrar las posiciones y pulsar ENTER para detener. La tabla de abajo enumera los valores NAME, MIN, POS y MAX de servos como shoulder_pan, shoulder_lift, elbow_flex, wrist_flex y gripper. Esta imagen se relaciona con la calibración del brazo Follower y presenta visualmente los parámetros durante la calibración.](../../en/images/d13-04.png)



## Notas

### ① Un brazo deja de moverse al alcanzar un límite

Hay que recalibrarlo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② No se encuentran los servos

![La imagen muestra la interfaz del terminal de Mac con un mensaje de error al ejecutar el código del robot de LeRobot. El error indica que la comprobación de motores de FeetechMotorsBus falló en el puerto '/dev/tty.usbmodem5AAF2193061', con los IDs de motor de -1 a -6 ausentes y un modelo esperado de 777. También enumera la lista completa de motores esperados y la lista completa de motores encontrados. Esta imagen se relaciona con la sección "Errores comunes" y presenta visualmente cómo aparece el problema de "no se encuentran los servos" como un error de tiempo de ejecución.](../../en/images/d13-05.png)

La alimentación de los servos no está conectada
