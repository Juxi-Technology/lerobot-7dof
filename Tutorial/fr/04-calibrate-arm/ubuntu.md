[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | Français | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Ordinateur Ubuntu

## Accorder les autorisations au port

Donnez à tous les utilisateurs la permission de lire et d'écrire sur ces périphériques série

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Calibrer le bras Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![L'image montre l'interface du terminal sur un ordinateur Ubuntu exécutant la commande « lerobot-calibrate » pour calibrer le bras Follower. Elle affiche les informations de connexion du Follower, les noms des articulations et les valeurs des limites supérieure et inférieure. Les informations clés incluent : appuyer sur Entrée pour démarrer la calibration, faire passer chaque articulation par ses limites supérieure et inférieure à tour de rôle, appuyer sur Entrée pour terminer la calibration ; ainsi que « Calibration saved to » et d'autres informations sur le chemin du fichier de calibration. Cette image est étroitement liée aux étapes de calibration du bras Follower et présente visuellement la rétroaction du terminal pendant la calibration.](../../en/images/d12-01.png)

## Calibrer le bras Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![L'image montre l'interface sur un ordinateur Ubuntu après l'octroi des autorisations au port. La ligne de commande a saisi « sudo chmod 666 /dev/ttyACM* » et, après exécution, a affiché des informations telles que « zihao_leader_arm ». En dessous figurent des invites telles que « press Enter to start calibration », « turn each joint through its upper and lower limits in turn » et « press Enter to finish calibration », ainsi que « Calibration saved to » et d'autres informations de chemin liées à la calibration. Cette image correspond à la section « Calibrer le bras Leader » et présente visuellement l'interface de préparation avant la calibration.](../../en/images/d12-02.png)

## Afficher le fichier de configuration de calibration

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![L'image montre le contenu du fichier « zihao_follower_arm.json » affiché dans le terminal Ubuntu. Le fichier contient les informations de configuration de plusieurs bras, tels que shoulder_pan, shoulder_lift, elbow_flex et wrist_flex, chaque bras ayant des paramètres comme id, drive_mode, homing_offset, range_min et range_max. Cette image concerne la section « Afficher le fichier de configuration de calibration » et présente visuellement les informations de paramètres spécifiques contenues dans le fichier de calibration, ce qui aide à comprendre la configuration de chaque bras.](../../en/images/d12-03.png)



## Remarques

### ① Un bras s'arrête de bouger après avoir atteint une limite

Il faut le recalibrer

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servomoteurs introuvables

![Cette capture d'écran montre une interface d'erreur dans le terminal Ubuntu, correspondant à la remarque « servomoteurs introuvables ». L'interface signale une RuntimeError, précisément « FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:' », c'est-à-dire que la vérification des servomoteurs a échoué. Elle liste aussi les informations de servomoteurs attendues, avec des ID de moteur attendus de 1 à 6 et un modèle attendu de 777, mais la liste des moteurs réellement trouvés est vide ; combinée au contexte, cette erreur est due au fait que les servomoteurs ne sont pas alimentés.](../../en/images/d12-04.png)

L'alimentation des servomoteurs n'est pas branchée
