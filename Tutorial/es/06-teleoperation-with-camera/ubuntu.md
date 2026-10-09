[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | Español | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Computadora con Ubuntu

## Conectar la cámara a la computadora

```Shell
lerobot-find-cameras opencv
```

![Esta imagen muestra el resultado de detección de cámaras en el terminal de Ubuntu. Se ejecutó el comando "lerobot-find-cameras opencv", que detectó una cámara numerada Camera #0 llamada OpenCV Camera con la ruta /dev/video0, tipo OpenCV y backend API V4L2; sus parámetros de formato de flujo predeterminados incluyen el formato Fourcc YUYV, ancho 640, alto 480 y velocidad de fotogramas 30.0. Por último muestra que el guardado de imágenes se completó y que las imágenes se almacenaron en el directorio outputs/captured_images. Esto corresponde al contenido sobre cómo encontrar las cámaras conectadas en una computadora con Ubuntu.](../../en/images/d20-01.png)

## Teleoperación con la imagen de la cámara mostrada

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

## Varias cámaras, teleoperación con las imágenes de las cámaras mostradas

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
