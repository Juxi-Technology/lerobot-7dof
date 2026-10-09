[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | Deutsch | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# Häufige Fehler und Behebungen

## Kameraaufnahme schlägt fehl

![Das Bild zeigt die Terminalausgabe beim Ausführen des LeRobot-Robotercodes. Die `INFO`-Logs zeigen unter anderem das Öffnen der OpenCV-Kamera und das Trennen des Follower; das `ERROR`-Log weist darauf hin, dass in der Datei `camera_opencv.py` die Funktion `read` wegen `OpenCVCamera(0) read failed` einen `RuntimeError` ausgelöst hat. Das Bild bezieht sich auf das Problem „Kameraaufnahme schlägt fehl" im Dokument und zeigt die beim Ausführen des Codes auftretende Fehlersituation, um das Verhalten des Problems in der Praxis zu verstehen.](../../en/images/d55-01.png)

Prüfen Sie, ob das Kabel der Handgelenkkamera locker ist, insbesondere das Ende nahe der Kamera – dieser Steckverbinder neigt stark zu schlechtem Kontakt

## Kamera trennt die Verbindung

![Das Bild zeigt den Code und die Fehlerinformationen des Problems „Kameraaufnahme schlägt fehl", das während des Laufs von LeRobot HF auftritt. Oben wird ein fehlgeschlagener Verbindungsaufbau zur OpenCV-Kamera angezeigt; darunter befindet sich der Fehler-Stack der Funktion `record_loop` in der Datei `lerobot_record.py`, der darauf hinweist, dass bei `async_read` ein `TimeoutError` auftrat und die Kamera innerhalb von 200 ms kein Frame geliefert hat. Dieses Bild entspricht dem Kontext des Problems „Kameraaufnahme schlägt fehl" und zeigt die konkrete Ausprägung des Problems auf Code-Ebene.](../../en/images/d55-02.png)

Starten Sie die Kommandozeile neu

## Servo-Kommunikationsproblem 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![Dieses Bild ist ein Terminal-Fehlerprotokoll und entspricht dem im Dokument erwähnten Inhalt zu „Servo-Kommunikationsproblem 1". Das Protokoll zeigt konkrete Codedateipfade: In den Zeilen 1069 und 1181 der Datei `lerobot/src/lerobot/motors/motors_bus.py` wird der Fehler `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries` protokolliert, außerdem die Fehlerursache `There is no status packet!`. Damit werden die Details des im Dokument genannten Fehlers „Servo-Kommunikationsproblem 1" vollständig dargestellt.](../../en/images/fix-04.png)

Die Lösung: Ändern Sie im Code unter `lerobot/src/lerobot/motors/motors_bus.py` jedes `num_retry` auf 99, insbesondere das in der Zeile, die den Fehler auslöst

![Das Bild zeigt den Inhalt der Codedatei `motors_bus.py` im LeRobot-Projekt. Die Methode `write` der Klasse `MotorsBusABC` ist hervorgehoben, wobei die Variable `num_retry` auf `99` geändert wurde. Das Bild bezieht sich auf den Abschnitt „Servo-Kommunikationsproblem 1" im Dokument und entspricht der Behebung, im Code unter `lerobot/src/lerobot/motors/motors_bus.py` jedes `num_retry` auf 99 zu ändern, insbesondere im Code der Zeile, die den Fehler auslöst.](../../en/images/d55-03.png)

## Servo-Kommunikationsproblem 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![Dieses Bild zeigt ein Fehlerprotokoll, das zum Fehlerinhalt „Servo-Kommunikationsproblem 2" im Dokument gehört. Konkret trat beim Ausführen des Codes ein Fehler namens ConnectionError auf, mit dem Verhalten Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries; die begleitende Fehlermeldung nennt zudem There is no status packet, und außerdem ist der Pfad des Codes angegeben, der den Fehler ausgelöst hat. Dieses Fehlerprotokoll entspricht genau der Beschreibung von „Servo-Kommunikationsproblem 2" im Dokument und ist die tatsächliche Fehleraufzeichnung dieses Problems.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![Das Bild zeigt eine Kommandozeilensitzung im zsh-Terminal unter macOS. Das Terminal zeigt mehrere Dateipfade und Codedateiinhalte; in der Datei `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` befindet sich die Fehlermeldung `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`. Dieses Bild bezieht sich auf den Inhalt zu „Servo-Kommunikationsproblem 2" und veranschaulicht die Terminaloberfläche im Moment des Fehlers.](../../en/images/fix-05.png)
</column>
</grid>

Lösung: Kalibrieren Sie den Roboterarm neu
