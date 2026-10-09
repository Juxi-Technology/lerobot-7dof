[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | Deutsch | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Mac-Rechner

## Die Kamera an den Computer anschließen

```Shell
lerobot-find-cameras opencv
```

![Das Bild zeigt das Erkennungsergebnis nach dem Anschließen einer Kamera an einen Mac. Es listet die Informationen zweier Kameras auf: die externe Kamera und die integrierte Frontkamera des Mac. Die Fps der externen Kamera betragen 60.00024 und die der integrierten Kamera 30.0. Dieses Bild gehört zum Inhalt über das Anschließen einer Kamera an einen Mac, veranschaulicht das Erkennungsergebnis nach dem Anschließen und hilft Anwendern, Typ, ID, Backend-API und weitere Informationen sowie die Bildrate jeder Kamera zu verstehen.](../../en/images/d21-01.png)

## Eine Kamera: Teleoperation mit angezeigtem Kamerabild

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

Nach der Ausführung startet die Teleoperation

Das rerun.io-Fenster öffnet sich und zeigt die Trajektorie jedes Servogelenks in Echtzeit zusammen mit dem Live-Kamerabild

und speichert die Bilder im Verzeichnis `~/username/outputs/captured_images`

![Dieses Bild der rerun.io-Oberfläche gehört zum Inhalt über die Teleoperation mit Kamera und ist die Echtzeit-Anzeige im Szenario der Ein-Kamera-Teleoperation. Links befindet sich der Liniendiagramm-Bereich, der die Trajektorienkurven der einzelnen Servogelenke zeigt, mit Legenden in unterschiedlichen Farben zur Unterscheidung der verschiedenen Gelenkdaten; rechts ist der Bereich in zwei übereinanderliegende Fenster aufgeteilt, die jeweils das Live-Kamerabild der Innenraumszene zeigen; unten befindet sich der Bereich für Zeitachse und Stream-Informationen. Dieses Szenario unterstützt außerdem das Speichern der von der Kamera erfassten Bilder im angegebenen Verzeichnis `~/username/outputs/captured_images`.](../../en/images/d21-02.png)

## Mehrere Kameras: Teleoperation mit angezeigten Kamerabildern

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
