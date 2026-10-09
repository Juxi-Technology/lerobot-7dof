[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | Français | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Ordinateur Mac

## Connecter la caméra à l'ordinateur

```Shell
lerobot-find-cameras opencv
```

![L'image montre le résultat de la détection après la connexion d'une caméra à un ordinateur Mac. Deux caméras sont listées : la caméra externe et la caméra frontale intégrée du Mac. Le Fps de la caméra externe est de 60.00024 et celui de la caméra intégrée de 30.0. Cette image concerne le contenu du document sur la connexion d'une caméra à un ordinateur Mac ; elle présente visuellement le résultat de la détection après la connexion, aidant l'utilisateur à comprendre le type, l'ID, l'API backend et d'autres informations de chaque caméra, ainsi que leur fréquence d'images.](../../en/images/d21-01.png)

## Une caméra, téléopération avec affichage du flux de la caméra

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

Après l'exécution, la téléopération démarre

La fenêtre rerun.io s'ouvre, affichant en temps réel la trajectoire de chaque articulation de servomoteur, ainsi que le flux vidéo de la caméra en direct

et enregistre les images dans le répertoire `~/username/outputs/captured_images`

![Cette image de l'interface rerun.io correspond au contenu du document sur la fonction de téléopération avec caméra et constitue l'affichage en temps réel dans le scénario de téléopération à une seule caméra. À gauche se trouve la zone de graphiques en courbes, qui montre les courbes d'évolution des trajectoires de chaque articulation de servomoteur, avec une légende marquée de différentes couleurs pour distinguer les données de chaque articulation ; à droite se trouvent deux fenêtres superposées, affichant respectivement le flux en direct de la scène intérieure filmée par la caméra ; en bas figure la zone de chronologie et d'informations de flux. Ce scénario permet aussi d'enregistrer les images capturées par la caméra dans le répertoire spécifié `~/username/outputs/captured_images`.](../../en/images/d21-02.png)

## Plusieurs caméras, téléopération avec affichage des flux des caméras

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
