[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | Français | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Ordinateur Ubuntu

## Accorder les autorisations au port

Donnez à tous les utilisateurs la permission de lire et d'écrire sur ces périphériques série

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Téléopération

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![L'image montre l'interface sur un ordinateur Ubuntu pour accorder les autorisations au port et pour la téléopération. D'abord, la commande `sudo chmod 666 /dev/ttyACM*` est exécutée pour accorder les autorisations aux périphériques série. Ensuite, la commande `lerobot-teleoperate` est saisie, affichant les informations du robot et du téléop, telles que l'id du robot « zihao follower arm » et le port « /dev/ttyACM0 », et l'id du téléop « zihao leader arm » et le port « /dev/ttyACM1 ». Cette image est étroitement liée au contenu sur l'octroi des autorisations de port et la téléopération, et présente visuellement l'opération et son résultat.](../../en/images/d16-01.png)
