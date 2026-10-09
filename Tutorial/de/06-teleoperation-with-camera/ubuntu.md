[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | Deutsch | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Ubuntu-Rechner

## Die Kamera an den Computer anschließen

```Shell
lerobot-find-cameras opencv
```

![Dieses Bild zeigt das Kameraerkennungsergebnis im Terminal eines Ubuntu-Rechners. Der Befehl „lerobot-find-cameras opencv" wurde ausgeführt und erkannte eine Kamera mit der Nummer Camera #0 und dem Namen OpenCV Camera, mit dem Pfad /dev/video0, dem Typ OpenCV und der Backend-API V4L2; ihre Standard-Stream-Formatparameter umfassen das Fourcc-Format YUYV, die Breite 640, die Höhe 480 und die Bildrate 30.0. Abschließend wird angezeigt, dass das Speichern der Bilder abgeschlossen ist und die Bilder im Verzeichnis outputs/captured_images abgelegt wurden. Dies entspricht dem Inhalt über das Finden angeschlossener Kameras auf einem Ubuntu-Rechner.](../../en/images/d20-01.png)

## Teleoperation mit angezeigtem Kamerabild

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

## Mehrere Kameras: Teleoperation mit angezeigten Kamerabildern

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
