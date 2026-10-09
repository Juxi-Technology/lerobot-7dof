[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | Italiano | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Computer Ubuntu

## Collegare la telecamera al computer

```Shell
lerobot-find-cameras opencv
```

![Questa immagine mostra il risultato di rilevamento delle telecamere nel terminale di Ubuntu. È stato eseguito il comando "lerobot-find-cameras opencv", che ha rilevato una telecamera numerata Camera #0 denominata OpenCV Camera con percorso /dev/video0, tipo OpenCV e API di backend V4L2; i suoi parametri di formato di stream predefiniti includono formato Fourcc YUYV, larghezza 640, altezza 480 e frequenza di fotogrammi 30.0. Infine mostra che il salvataggio delle immagini è completato e che le immagini sono state memorizzate nella directory outputs/captured_images. Questo corrisponde al contenuto sulla ricerca delle telecamere collegate su un computer Ubuntu.](../../en/images/d20-01.png)

## Teleoperazione con il feed della telecamera visualizzato

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

## Più telecamere, teleoperazione con i feed delle telecamere visualizzati

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
