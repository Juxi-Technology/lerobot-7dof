[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | Français | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Ordinateur Windows



<callout emoji="🚫">
Les bras Leader et Follower doivent tous deux être connectés
</callout>

## Calibrer le bras Follower

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![L'image montre l'interface en ligne de commande sur un ordinateur Windows exécutant une calibration lerobot. La commande est « lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm ». L'interface affiche les informations de calibration, notamment des invites telles que « zihao_follower_arm SO10IFollower connected », et liste aussi les valeurs NAME, MIN, POS et MAX de chaque articulation du bras robotisé. Pendant la calibration, elle invite l'utilisateur à déplacer le bras robotisé au milieu de sa plage de mouvement et à appuyer sur ENTRÉE, tout en enregistrant les positions, puis à appuyer sur ENTRÉE pour arrêter. Cette image concerne la calibration du bras Follower et montre les étapes concrètes et la rétroaction de l'interface.](../../en/images/d14-01.png)

## Calibrer le bras Leader

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![L'image montre l'interface en ligne de commande calibrant le bras robotisé avec la commande lerobot-calibrate sur un ordinateur Windows. Elle affiche les informations de calibration des bras Follower et Leader, notamment le chemin d'enregistrement de la position de calibration, le type de robot, le numéro de port et l'ID. Elle invite aussi à déplacer le Follower au milieu de sa plage de mouvement et à appuyer sur ENTRÉE, à faire parcourir à chaque articulation toute sa plage de mouvement, à enregistrer les positions et à appuyer sur ENTRÉE pour arrêter. En bas, elle affiche le nom, la valeur minimale, la position actuelle et la valeur maximale de chaque articulation.](../../en/images/d14-02.png)

## Où les fichiers sont exportés

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## Calibrer un autre bras robotisé

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## Remarques

### ① Un bras s'arrête de bouger après avoir atteint une limite

Il faut le recalibrer

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servomoteurs introuvables

![L'image montre un message d'erreur lors de l'exécution du programme lerobot sous macOS. Pendant l'exécution du programme, une RuntimeError apparaît : « FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061' », indiquant des ID de servomoteurs manquants, dont les servomoteurs 1 à 6, tous avec un numéro de modèle attendu de 777, mais la liste des servomoteurs réellement trouvés est vide. Cela concerne la remarque « servomoteurs introuvables » ; il se peut que les servomoteurs ne soient pas branchés, alors rebranchez-les et faites pivoter le connecteur.](../../en/images/d14-03.png)

L'alimentation des servomoteurs n'est pas branchée ; rebranchez-la et faites pivoter le connecteur
