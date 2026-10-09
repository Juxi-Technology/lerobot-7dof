[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | Deutsch | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Windows-Rechner



<callout emoji="🚫">
Der Leader- und der Follower-Arm müssen beide angeschlossen sein
</callout>

## Den Follower-Arm kalibrieren

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![Das Bild zeigt die Kommandozeilenoberfläche auf einem Windows-Rechner bei Ausführung einer lerobot-Kalibrierung. Der Befehl lautet „lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm". Die Oberfläche zeigt Kalibrierinformationen, einschließlich Hinweisen wie „zihao_follower_arm SO10IFollower connected", und listet außerdem die Werte NAME, MIN, POS und MAX für jedes Gelenk des Roboterarms auf. Während der Kalibrierung werden Sie aufgefordert, den Roboterarm in die Mitte seines Bewegungsbereichs zu bewegen und ENTER zu drücken, während die Positionen aufgezeichnet werden, und ENTER zu drücken, um zu stoppen. Dieses Bild gehört zum Kalibrieren des Follower-Arms und zeigt die konkreten Schritte und die Rückmeldung der Oberfläche.](../../en/images/d14-01.png)

## Den Leader-Arm kalibrieren

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![Das Bild zeigt die Kommandozeilenoberfläche beim Kalibrieren des Roboterarms mit dem Befehl lerobot-calibrate auf einem Windows-Rechner. Es zeigt Informationen zur Kalibrierung des Follower- und des Leader-Arms, einschließlich des Speicherpfads der Kalibrierungsposition, Robotertyp, Portnummer und ID. Außerdem werden Sie aufgefordert, den Follower in die Mitte seines Bewegungsbereichs zu bewegen und ENTER zu drücken, jedes Gelenk durch seinen vollständigen Bewegungsbereich zu führen, die Positionen aufzuzeichnen und ENTER zu drücken, um zu stoppen. Unten werden Name, Minimalwert, aktuelle Position und Maximalwert jedes Gelenks angezeigt.](../../en/images/d14-02.png)

## Wohin die Dateien exportiert werden

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## Einen anderen Roboterarm kalibrieren

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## Hinweise

### ① Ein Arm bleibt nach Erreichen einer Grenze stehen

Er muss neu kalibriert werden

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos nicht gefunden

![Das Bild zeigt eine Fehlermeldung beim Ausführen des lerobot-Programms unter macOS. Während das Programm läuft, erscheint ein RuntimeError: „FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'", was auf fehlende Servo-IDs hindeutet, darunter die Servos 1-6, alle mit einer erwarteten Modellnummer 777, während die Liste der tatsächlich gefundenen Servos leer ist. Dies bezieht sich auf den Hinweis „Servos nicht gefunden"; möglicherweise sind die Servos nicht angeschlossen, also schließen Sie sie erneut an und drehen Sie den Stecker.](../../en/images/d14-03.png)

Die Servo-Stromversorgung ist nicht angeschlossen; schließen Sie sie erneut an und drehen Sie den Stecker
