[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | Deutsch | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Mac-Rechner

## Die Portnummern überprüfen

Follower-Arm:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader-Arm:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Den Follower-Arm kalibrieren

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![Das Bild zeigt die Kommandozeilenoberfläche zum Kalibrieren der SO101-Servos auf einem Mac. Der Befehl lautet „lerobot-calibrate", mit Parametern wie robot.type, robot.port und robot.id. Die Oberfläche zeigt Roboter-Konfigurationsinformationen wie „zihao_follower_arm". Darunter werden Sie aufgefordert, „c" und Enter zu drücken, um die Kalibrierung zu starten, und es werden Meldungen wie „zihao_follower_arm SO101Follower connected" angezeigt. Dieses Bild entspricht dem Abschnitt „Den Follower-Arm kalibrieren" und veranschaulicht den Kalibrierbefehl und die Rückmeldung der Oberfläche.](../../en/images/d13-01.png)

![Das Bild zeigt die Kommandozeilenoberfläche für einen LeRobot-Kalibriervorgang unter Ubuntu. Der Befehl lautet „lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm" und zeigt die Kalibrierinformationen des Follower, einschließlich Minimal-, Maximal- und aktueller Position jedes Gelenks. Wichtige Bedienhinweise sind rot umrandet hervorgehoben, etwa „press Enter to start calibration", „turn each joint through its upper and lower limits in turn" und „press Enter to finish calibration", was die im Kontext beschriebenen Kalibrierschritte aufgreift.](../../en/images/d13-02.png)

## Den Leader-Arm kalibrieren

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![Das Bild zeigt die Kommandozeilenoberfläche für eine LeRobot-Kalibrierung unter Ubuntu. In der Kommandozeile wurden Vorgänge wie „sudo chmod 666 /dev/ttyACM*" und „lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1" ausgeführt, wobei die Portnummerinformationen des Follower- und des Leader-Arms angezeigt werden. Die Oberfläche fordert Sie außerdem auf, Enter zu drücken, um die Kalibrierung zu starten, jedes Gelenk nacheinander durch seine oberen und unteren Grenzen zu bewegen und Enter zu drücken, um abzuschließen, und zeigt schließlich den Pfad an, unter dem die Kalibrierungskonfigurationsdatei gespeichert wird. Dieses Bild gehört zum Inhalt der LeRobot-Kalibrierung und veranschaulicht die Kalibrierschritte.](../../en/images/d13-03.png)

## Die Kalibrierungskonfigurationsdatei ansehen

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## Häufige Fehler

- Einer oder mehrere der Servos können nicht gefunden werden

![Das Bild zeigt die während der Kalibrierung des SO-Follower angezeigten Servo-Parameterinformationen. Oben sind Verbindungsinformationen und ein Kalibrierhinweis zu sehen, der Sie auffordert, den Follower in die Mitte seines Bewegungsbereichs zu bewegen und ENTER zu drücken, dann alle Gelenke der Reihe nach durch ihren Bewegungsbereich zu führen, die Positionen aufzuzeichnen und ENTER zu drücken, um zu stoppen. Die Tabelle darunter listet die Werte NAME, MIN, POS und MAX für Servos wie shoulder_pan, shoulder_lift, elbow_flex, wrist_flex und gripper auf. Dieses Bild gehört zum Kalibrieren des Follower-Arms und veranschaulicht die Parameter während der Kalibrierung.](../../en/images/d13-04.png)



## Hinweise

### ① Ein Arm bleibt nach Erreichen einer Grenze stehen

Er muss neu kalibriert werden

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos nicht gefunden

![Das Bild zeigt die Mac-Terminaloberfläche mit einer Fehlermeldung aus dem Ausführen des LeRobot-Robotercodes. Der Fehler besagt, dass die Motorprüfung von FeetechMotorsBus auf dem Port „/dev/tty.usbmodem5AAF2193061" fehlgeschlagen ist, wobei die Motor-IDs -1 bis -6 fehlen und das erwartete Modell 777 ist. Außerdem werden die vollständige Liste der erwarteten Motoren und die vollständige Liste der gefundenen Motoren aufgeführt. Dieses Bild gehört zum Abschnitt „Häufige Fehler" und veranschaulicht, wie sich das Problem „Servos nicht gefunden" als Laufzeitfehler äußert.](../../en/images/d13-05.png)

Die Servo-Stromversorgung ist nicht angeschlossen
