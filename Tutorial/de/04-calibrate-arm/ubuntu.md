[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | Deutsch | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Ubuntu-Rechner

## Berechtigungen für den Port erteilen

Geben Sie allen Benutzern Lese- und Schreibberechtigung für diese seriellen Geräte

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Den Follower-Arm kalibrieren

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![Das Bild zeigt die Terminaloberfläche eines Ubuntu-Rechners bei Ausführung des Befehls „lerobot-calibrate" zum Kalibrieren des Follower-Arms. Es zeigt die Verbindungsinformationen des Follower, Gelenknamen und Ober-/Untergrenzwerte. Zu den wichtigsten Informationen gehören: Enter drücken, um die Kalibrierung zu starten, jedes Gelenk nacheinander durch seine oberen und unteren Grenzen bewegen, Enter drücken, um die Kalibrierung abzuschließen; sowie „Calibration saved to" und weitere Pfadinformationen zur Kalibrierungsdatei. Dieses Bild steht in engem Zusammenhang mit den Schritten zum Kalibrieren des Follower-Arms und veranschaulicht die Terminalrückmeldung während der Kalibrierung.](../../en/images/d12-01.png)

## Den Leader-Arm kalibrieren

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![Das Bild zeigt die Oberfläche eines Ubuntu-Rechners nach dem Erteilen von Berechtigungen für den Port. In der Kommandozeile wurde „sudo chmod 666 /dev/ttyACM*" eingegeben, und nach der Ausführung wurden Informationen wie „zihao_leader_arm" angezeigt. Darunter stehen Hinweise wie „press Enter to start calibration", „turn each joint through its upper and lower limits in turn" und „press Enter to finish calibration", zusammen mit „Calibration saved to" und weiteren kalibrierungsbezogenen Pfadinformationen. Dieses Bild entspricht dem Abschnitt „Den Leader-Arm kalibrieren" und veranschaulicht die Vorbereitungsoberfläche vor der Kalibrierung.](../../en/images/d12-02.png)

## Die Kalibrierungskonfigurationsdatei ansehen

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![Das Bild zeigt den Inhalt der Datei „zihao_follower_arm.json", wie er im Ubuntu-Terminal angezeigt wird. Die Datei enthält Konfigurationsinformationen für mehrere Arme wie shoulder_pan, shoulder_lift, elbow_flex und wrist_flex, wobei jeder Arm Parameter wie id, drive_mode, homing_offset, range_min und range_max besitzt. Dieses Bild gehört zum Abschnitt „Die Kalibrierungskonfigurationsdatei ansehen" und zeigt die konkreten Parameterinformationen in der Kalibrierungsdatei, um Anwendern das Verständnis der Konfiguration jedes Arms zu erleichtern.](../../en/images/d12-03.png)



## Hinweise

### ① Ein Arm bleibt nach Erreichen einer Grenze stehen

Er muss neu kalibriert werden

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos nicht gefunden

![Dies ist ein Screenshot, der eine Fehleroberfläche im Ubuntu-Terminal zeigt, entsprechend dem Hinweis „Servos nicht gefunden". Die Oberfläche meldet einen RuntimeError, konkret „FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'", d. h. die Servoprüfung ist fehlgeschlagen. Außerdem werden die erwarteten Servoinformationen aufgelistet, mit erwarteten Motor-IDs 1-6 und dem erwarteten Modell 777, während die Liste der tatsächlich gefundenen Motoren leer ist; im Zusammenhang mit dem Kontext wird dieser Fehler dadurch verursacht, dass die Servos nicht mit Strom versorgt werden.](../../en/images/d12-04.png)

Die Servo-Stromversorgung ist nicht angeschlossen
