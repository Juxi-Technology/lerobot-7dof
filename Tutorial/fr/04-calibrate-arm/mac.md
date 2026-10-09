[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | Français | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Ordinateur Mac

## Vérifier les numéros de port

Bras Follower :

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Bras Leader :

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Calibrer le bras Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![L'image montre l'interface en ligne de commande pour calibrer les servomoteurs SO101 sur un Mac. La commande est « lerobot-calibrate », avec des paramètres dont robot.type, robot.port et robot.id. L'interface affiche les informations de configuration du robot telles que « zihao_follower_arm ». En dessous, elle vous invite à appuyer sur « c » puis Entrée pour démarrer la calibration, et affiche aussi des messages tels que « zihao_follower_arm SO101Follower connected ». Cette image correspond à la section « Calibrer le bras Follower » et présente visuellement la commande de calibration et la rétroaction de l'interface.](../../en/images/d13-01.png)

![L'image montre l'interface en ligne de commande d'une opération de calibration LeRobot sous Ubuntu. La commande est « lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm », affichant les informations de calibration du Follower, notamment la position minimale, maximale et actuelle de chaque articulation. Les invites d'opération clés sont mises en évidence par des cadres rouges, telles que « press Enter to start calibration », « turn each joint through its upper and lower limits in turn » et « press Enter to finish calibration », reprenant les étapes de calibration décrites dans le contexte.](../../en/images/d13-02.png)

## Calibrer le bras Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![L'image montre l'interface en ligne de commande d'une calibration LeRobot sous Ubuntu. La ligne de commande a exécuté des opérations telles que « sudo chmod 666 /dev/ttyACM* » et « lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1 », affichant les informations de numéro de port des bras Follower et Leader. L'interface vous invite également à appuyer sur Entrée pour démarrer la calibration, à faire passer chaque articulation par ses limites supérieure et inférieure à tour de rôle, et à appuyer sur Entrée pour terminer, et affiche enfin le chemin où le fichier de configuration de calibration est enregistré. Cette image concerne le contenu de la calibration LeRobot et présente visuellement les étapes de calibration.](../../en/images/d13-03.png)

## Afficher le fichier de configuration de calibration

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## Problèmes courants

- Un ou plusieurs des servomoteurs sont introuvables

![L'image montre les informations sur les paramètres des servomoteurs affichées pendant la calibration du Follower SO. En haut figurent les informations de connexion et une invite de calibration vous demandant de déplacer le Follower au milieu de sa plage de mouvement et d'appuyer sur ENTRÉE, puis de faire parcourir à toutes les articulations leur plage de mouvement dans l'ordre, d'enregistrer les positions et d'appuyer sur ENTRÉE pour arrêter. Le tableau ci-dessous liste les valeurs NAME, MIN, POS et MAX pour des servomoteurs tels que shoulder_pan, shoulder_lift, elbow_flex, wrist_flex et gripper. Cette image concerne la calibration du bras Follower et présente visuellement les paramètres pendant la calibration.](../../en/images/d13-04.png)



## Remarques

### ① Un bras s'arrête de bouger après avoir atteint une limite

Il faut le recalibrer

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servomoteurs introuvables

![L'image montre l'interface du terminal Mac avec un message d'erreur issu de l'exécution du code du robot LeRobot. L'erreur indique que la vérification des moteurs FeetechMotorsBus a échoué sur le port « /dev/tty.usbmodem5AAF2193061 », les ID de moteur -1 à -6 étant manquants et le modèle attendu étant 777. Elle liste également la liste complète des moteurs attendus et la liste complète des moteurs trouvés. Cette image concerne la section « Problèmes courants » et présente visuellement la façon dont le problème « servomoteurs introuvables » se manifeste sous forme d'erreur d'exécution.](../../en/images/d13-05.png)

L'alimentation des servomoteurs n'est pas branchée
