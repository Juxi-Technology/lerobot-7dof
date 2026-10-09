[English](../en/05-teleoperation/mac.md) | [简体中文](../zh-hans/05-teleoperation/mac.md) | [繁體中文](../zh-hant/05-teleoperation/mac.md) | [Deutsch](../de/05-teleoperation/mac.md) | [Español](../es/05-teleoperation/mac.md) | Français | [Italiano](../it/05-teleoperation/mac.md) | [日本語](../ja/05-teleoperation/mac.md) | [한국어](../ko/05-teleoperation/mac.md) | [Português (BR)](../pt-br/05-teleoperation/mac.md) | [Português (PT)](../pt-pt/05-teleoperation/mac.md)

# Ordinateur Mac

## Accorder les autorisations au port

Donnez à tous les utilisateurs la permission de lire et d'écrire sur ces périphériques série

```Shell
chmod 666 /dev/tty.*
```

## Vérifier les numéros de port

Bras Follower :

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Bras Leader :

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Téléopération

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm
```

## L'autre port fonctionne aussi

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.wchusbserial5AAF2193061 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.wchusbserial5AAF2194741 \
    --teleop.id=my_leader_arm
```
