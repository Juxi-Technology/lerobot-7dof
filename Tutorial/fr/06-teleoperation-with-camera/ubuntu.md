[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | Français | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Ordinateur Ubuntu

## Connecter la caméra à l'ordinateur

```Shell
lerobot-find-cameras opencv
```

![Cette image montre le résultat de la détection des caméras dans le terminal Ubuntu. La commande « lerobot-find-cameras opencv » a été exécutée, détectant une caméra numérotée Camera #0 et nommée OpenCV Camera avec le chemin /dev/video0, le type OpenCV et l'API backend V4L2 ; ses paramètres de format de flux par défaut incluent le format Fourcc YUYV, une largeur de 640, une hauteur de 480 et une fréquence d'images de 30.0. Enfin, elle indique que l'enregistrement des images est terminé et que les images ont été stockées dans le répertoire outputs/captured_images. Cela correspond au contenu sur la recherche des caméras connectées sur un ordinateur Ubuntu.](../../en/images/d20-01.png)

## Téléopération avec affichage du flux de la caméra

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

## Plusieurs caméras, téléopération avec affichage des flux des caméras

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
