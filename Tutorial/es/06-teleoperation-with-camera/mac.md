[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | Español | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Computadora con Mac

## Conectar la cámara a la computadora

```Shell
lerobot-find-cameras opencv
```

![La imagen muestra el resultado de la detección tras conectar una cámara a un Mac. En la pantalla se enumeran los datos de dos cámaras: la cámara externa y la cámara frontal integrada del Mac. La Fps de la cámara externa es 60.00024 y la Fps de la cámara integrada es 30.0. Esta imagen se relaciona con el contenido que describe la conexión de una cámara a un Mac y presenta visualmente el resultado de la detección tras la conexión, ayudando al usuario a comprender el tipo, el ID, la API de backend, etc. de cada cámara, así como su velocidad de fotogramas.](../../en/images/d21-01.png)

## Una cámara, teleoperación con la imagen de la cámara mostrada

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

Tras la ejecución, comienza la teleoperación

Se abre la ventana de rerun.io, que muestra en tiempo real la trayectoria de cada articulación del servo, junto con la imagen en vivo de la cámara

y guarda las imágenes en el directorio `~/username/outputs/captured_images`

![Esta imagen es la interfaz de rerun.io y corresponde al contenido sobre la función de teleoperación con cámara; es la interfaz de visualización en tiempo real en el escenario de teleoperación con una sola cámara. En el lado izquierdo de la interfaz está el área de gráficos de líneas, que muestra las curvas de variación de la trayectoria de cada articulación del servo, con una leyenda de distintos colores que distingue los diversos datos de las articulaciones; el lado derecho se divide en dos ventanas, una arriba y otra abajo, que muestran respectivamente la imagen en tiempo real del interior captada por la cámara; en la parte inferior está el área de la línea de tiempo y de la información de streams, y además este escenario permite guardar las imágenes capturadas por la cámara en el directorio especificado `~/username/outputs/captured_images`.](../../en/images/d21-02.png)

## Varias cámaras, teleoperación con las imágenes de las cámaras mostradas

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
