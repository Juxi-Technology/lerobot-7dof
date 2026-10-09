[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | Deutsch | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Ubuntu-Rechner

## Berechtigungen für den Port erteilen

Geben Sie allen Benutzern Lese- und Schreibberechtigung für diese seriellen Geräte

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Teleoperation

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![Das Bild zeigt die Oberfläche eines Ubuntu-Rechners zum Erteilen von Berechtigungen für den Port und zur Teleoperation. Zuerst wird der Befehl `sudo chmod 666 /dev/ttyACM*` ausgeführt, um den seriellen Geräten Berechtigungen zu erteilen. Dann wird der Befehl `lerobot-teleoperate` eingegeben, der die Roboter- und Teleop-Informationen zeigt, etwa die Roboter-ID „zihao follower arm" und den Port „/dev/ttyACM0" sowie die Teleop-ID „zihao leader arm" und den Port „/dev/ttyACM1". Dieses Bild steht in engem Zusammenhang mit dem Inhalt zum Erteilen von Portberechtigungen und zur Teleoperation und veranschaulicht den Vorgang und sein Ergebnis.](../../en/images/d16-01.png)
