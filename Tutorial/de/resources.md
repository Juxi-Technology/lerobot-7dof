[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | Deutsch | [Español](../es/resources.md) | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>Einige Ressourcen</title>

# Lerbots offizielle [URDF-Datei](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf)



## URDF Studio

https://urdf.d-robotics.cc/



## ROS2-Simulationssteuerung (selbst umsetzen)

https://github.com/holmsslk/so-arm-moveit-hardware



## Die offizielle grafische Oberfläche von LeRobot

https://github.com/huggingface/leLab

LeLab ist eine Web-App, die den gesamten LeRobot-Workflow – Kalibrierung, Teleoperation, Aufnahme, Training, Wiedergabe – in einer einzigen Browser-Oberfläche vereint. Einfach den Roboterarm anschließen, die App öffnen und schon kann es losgehen. Keine umständliche Kommandozeilenarbeit und keine Tastatureingabe nötig.

🤗 Der native Web-Einstiegspunkt von LeRobot, der neue Nutzer in wenigen Minuten von „out of the box“ bis zum „Training ihrer ersten Policy“ bringt.

🤗 Installation und Ausführung von allem mit einem einzigen Befehl.



# Den Follower-Arm vom Handy aus steuern

https://huggingface.co/docs/lerobot/main/en/phone_teleop




## Cloud-Robotik-Entwicklung: ROS-2-Geräte sowie Isaac-Sim-LeRobot-Simulation und Datenstreaming auf AWS

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## Servo-IDs und Mittelpunkt-Kalibrierung in der Web-Oberfläche einstellen

https://bambot.org/feetech.js?lang=zh

1. Geben Sie je nach Servo-Modell 0 oder 1 ein und klicken Sie dann auf „Connect“

![Das Bild zeigt die Verbindungsoberfläche beim Einstellen der Servo-IDs und der Mittelpunkt-Kalibrierung in der Web-Oberfläche. Die Oberfläche enthält einen Abschnitt „Connect“ mit einem Baudrate-Dropdown, das aktuell auf „1,000,000 bps (Index 0)“ steht; ein Dropdown für das Protokollende, aktuell auf „0=STS/SMS“ gesetzt; und eine Schaltfläche „Connect“ (grün, mit der Zahl 2 gekennzeichnet). Unten zeigt die Oberfläche den Status (rot, „Disconnected“). Das Bild schließt an den oben genannten Kontext an: Nachdem Sie je nach Servo-Modell 0 oder 1 eingegeben und auf „Connect“ geklickt haben, werden Servos gescannt, um den passenden ID-Servo zu bestätigen; dies ist eine zentrale Oberfläche in diesem Ablauf.](../en/images/d58-01.png)

2. Servos mit den IDs 1\~6 scannen; anhand von FOUND in den Scan-Ergebnissen den passenden ID-Servo bestätigen. Im Bild wurde zum Beispiel Servo-ID 1 gefunden

![Dies ist ein Screenshot der Servo-Scan-Oberfläche in der Lerbot-Software; im Kern geht es um den Ablauf und die Ergebnisse des Servo-Scannens. Die Oberfläche hat die Eingabefelder „Start ID“ und „End ID“, in die 1 bzw. 6 eingegeben wurden, sowie eine Schaltfläche „Start scan“; darunter befindet sich der zugehörige Anzeigebereich für die Scan-Ergebnisse, passend zum im Dokument erwähnten Inhalt über gefundene Servo-IDs. Das Protokoll der Scan-Ergebnisse zeigt, dass Servoinformationen nur beim Scannen von ID1 erfolgreich abgerufen wurden, mit der Anzeige „FOUND: Pos-1230, Mode=0, BaudIdx=0“, während beim Scannen von ID2 bis ID6 jeweils Fehlermeldungen über eine ungewöhnliche Positionslesung und ein fehlendes Statuspaket auftraten.](../en/images/d58-02.png)

3. ID-Einstellung und Mittelpunkt-Kalibrierung

① Setzen Sie die Eingabe der aktuellen Servo-ID auf die ID des gescannten Servos

② Geben Sie unter „ID management“ eine Zahl ein und klicken Sie auf „Change ID“, um die ID festzulegen

③ Mittelpunkt-Kalibrierung (der Mittelpunkt des STS3215-Servos ist 2047, der des SCS0009-Servos ist 511)

STS-Servo: Geben Sie 2047 unter „Position control“ ein und klicken Sie auf „Set“

SCS-Servo: Geben Sie 511 unter „Position control“ ein und klicken Sie auf „Set“

![Das Bild zeigt die Oberfläche zur Steuerung eines einzelnen Servos in Lerbots offiziellem URDF Studio. Die „Current servo ID“ ist mit 1 angegeben; darunter können Sie unter „ID management“ eine Zahl eingeben und auf „Change ID“ klicken; im Bereich „Position Control“ gibt es eine Schaltfläche „Read position“, die die Position 2047 neben einer Schaltfläche „Set“ anzeigt. Dieses Bild schließt eng an den Kontext an und zeigt die dort genannten Schritte: die Eingabe der aktuellen Servo-ID auf die ID des gescannten Servos setzen, unter „ID management“ eine Zahl eingeben und auf „Change ID“ klicken zum Festlegen der ID, und beim STS-Servo 2047 unter „Position control“ eingeben und auf „Set“ klicken.](../en/images/d58-03.png)
