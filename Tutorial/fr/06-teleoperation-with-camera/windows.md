[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | Français | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Ordinateur Windows

## Connecter la caméra à l'ordinateur

```Shell
lerobot-find-cameras opencv
```

![Cette image est l'écran d'erreur en ligne de commande sous Windows, correspondant au scénario d'erreur « la caméra ne peut pas se connecter » mentionné dans le document. Les informations d'erreur affichées comportent deux parties : une erreur globale « Camera index out of range » et une erreur du script lié à lerobot « Failed to connect or configure OpenCV camera 0 ». L'interface liste aussi les caméras détectées : 2 périphériques nommés OpenCV Camera, tous deux avec le backend DSHOW, au format YUY2, largeur 640, hauteur 480, fréquence d'images 0.0 FPS. Ce contenu d'erreur correspond à la situation décrite dans le document, où la caméra ne peut pas se connecter mais s'ouvre toujours normalement lorsqu'on change de caméra dans Tencent Meeting.](../../en/images/d22-01.png)

## Téléopération avec affichage du flux de la caméra

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

La fenêtre rerun.io s'ouvre, affichant en temps réel la trajectoire de chaque articulation de servomoteur, ainsi que le flux vidéo de la caméra en direct

et enregistre les images dans le répertoire `C:\Users\username\outputs\captured_images`

![L'image montre la fenêtre rerun.io, affichant en temps réel les trajectoires de chaque articulation de servomoteur et le flux vidéo de la caméra en direct. À gauche se trouve l'interface de blueprint, avec des options telles que « teleoperation ». Au milieu se trouve le graphique de trajectoire, affichant des données de position d'articulation telles que « observation_wrist_rot.pos ». À droite se trouve le flux vidéo de la caméra, montrant la scène du point de vue du robot. En haut à droite s'affiche « Waiting for data on rerun: http://127.0.0.1:9876/remote... », avec les informations de source de données en dessous. Cette image concerne le contenu décrivant la fenêtre rerun.io montrant le flux vidéo de la caméra en temps réel, et présente visuellement le rendu.](../../en/images/d22-02.png)

## Si vous rencontrez l'erreur suivante

La caméra ne peut pas se connecter, mais changer de caméra dans Tencent Meeting s'ouvre toujours normalement

![L'image montre l'interface de ligne de commande Windows, affichant les résultats de détection des caméras. En haut figure « Administrateur : Invite de commandes - C:\Users\40743\miniconda3\condabin\conda.bat activate leroobot ». En dessous, la sortie « Detected Cameras » liste « Camera #0 » comme OpenCV Camera @ 0, avec Type OpenCV, Backend api DSHOW, Default stream profile et d'autres paramètres, et affiche aussi des messages d'erreur tels que « ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0) ». Cette image se rapporte au contexte du problème de connexion de la caméra dans le document et présente visuellement les informations d'erreur correspondantes.](../../en/images/d22-03.png)

Modifiez le fichier `lerobot\src\lerobot\cameras\utils.py` pour remplacer le backend OpenCV par `cv2.CAP_SHOW`

![L'image montre un extrait de code Python servant à modifier le fichier `utils.py` de la bibliothèque lerobot, correspondant à l'étape de dépannage de l'erreur de connexion de la caméra sous Windows décrite dans le document. Le code définit une fonction `get_cv2_backend()` qui configure le backend OpenCV selon le système : lorsque le système est Windows, le `return int(cv2.CAP_MSMF)` commenté à l'origine est commenté et le contenu actuellement surligné est `return int(cv2.CAP_DSHOW)`, avec une annotation à côté expliquant que cette solution remplace AVFOUNDATION ; la fonction configure aussi le backend OpenCV correspondant pour macOS et Linux. C'est la modification de code essentielle pour résoudre le problème de connexion de la caméra spécifié.](../../en/images/d22-04.png)

> C'est un bug que même Doubao ne peut pas résoudre ; c'est uniquement parce que la bibliothèque lerobot est trop profondément encapsulée, et il est très difficile pour les débutants de déboguer

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## Connecter plusieurs caméras, téléopération avec affichage des flux des caméras

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
