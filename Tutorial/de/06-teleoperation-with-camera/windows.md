[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | Deutsch | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Windows-Rechner

## Die Kamera an den Computer anschließen

```Shell
lerobot-find-cameras opencv
```

![Dieses Bild ist die Kommandozeilen-Fehleroberfläche von Windows und entspricht dem im Dokument genannten Fehlerszenario „Die Kamera kann keine Verbindung herstellen". Die angezeigte Fehlermeldung besteht aus zwei Teilen: einem globalen Fehler „Camera index out of range" und einem lerobot-bezogenen Skriptfehler „Failed to connect or configure OpenCV camera 0". Die Oberfläche listet außerdem die erkannten Kameras auf: insgesamt 2 Geräte mit dem Namen OpenCV Camera, beide mit dem DSHOW-Backend, Format YUY2, Breite 640, Höhe 480, Bildrate 0.0FPS. Dieser Fehlerinhalt entspricht der im Dokument beschriebenen Situation, dass die Kamera keine Verbindung herstellen kann, in Tencent Meeting aber trotzdem normal funktioniert.](../../en/images/d22-01.png)

## Teleoperation mit angezeigtem Kamerabild

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

Das rerun.io-Fenster öffnet sich und zeigt die Trajektorie jedes Servogelenks in Echtzeit zusammen mit dem Live-Kamerabild

und speichert die Bilder im Verzeichnis `C:\Users\username\outputs\captured_images`

![Das Bild zeigt das rerun.io-Fenster, das die Trajektorien der Servogelenke und das Live-Kamerabild in Echtzeit anzeigt. Links befindet sich die Blueprint-Oberfläche mit Optionen wie „teleoperation". In der Mitte ist das Trajektoriendiagramm, das Gelenkpositionsdaten wie „observation_wrist_rot.pos" zeigt. Rechts ist das Kamerabild, das die Szene aus der Perspektive des Roboters zeigt. Oben rechts steht „Waiting for data on rerun: http://127.0.0.1:9876/remote...", darunter Informationen zur Datenquelle. Dieses Bild gehört zum Inhalt, der das rerun.io-Fenster mit dem Echtzeit-Kamerabild beschreibt, und veranschaulicht den Effekt.](../../en/images/d22-02.png)

## Wenn der folgende Fehler auftritt

Die Kamera kann keine Verbindung herstellen, aber beim Wechseln der Kamera in Tencent Meeting öffnet sie sich trotzdem normal

![Das Bild zeigt die Windows-Kommandozeilenoberfläche mit Kameraprüfergebnissen. Oben steht „Administrator: Eingabeaufforderung - C:\Users\40743\miniconda3\condabin\conda.bat activate leroobot". Darunter wird die Ausgabe „Detected Cameras" angezeigt, mit „Camera #0" als OpenCV Camera @ 0, Type OpenCV, Backend api DSHOW, Default stream profile und weiteren Parametern, sowie Fehlermeldungen wie „ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)". Dieses Bild hängt mit dem Kontext des Kameraverbindungsproblems im Dokument zusammen und veranschaulicht die zugehörigen Fehlerinformationen.](../../en/images/d22-03.png)

Ändern Sie die Datei `lerobot\src\lerobot\cameras\utils.py`, um das OpenCV-Backend auf `cv2.CAP_SHOW` umzustellen

![Das Bild zeigt den Code der Funktion `get_cv2_backend()` in der Datei `lerobot\src\lerobot\cameras\utils.py`. Wenn das System Windows ist, ist das ursprünglich auskommentierte `return int(cv2.CAP_MSMF)` auskommentiert, und der aktuell hervorgehobene Inhalt ist `return int(cv2.CAP_DSHOW)`, daneben der Hinweis, dass dieser Ansatz AVFOUNDATION ersetzt. Die Funktion legt außerdem für macOS und Linux die entsprechenden OpenCV-Backends fest. Dieses Bild gehört zum Vorgang des Änderns der Datei `lerobot\src\lerobot\cameras\utils.py`, um das OpenCV-Backend zu `cv2.CAP_SHOW` zu ändern, und ist die zentrale Codeänderung zur Behebung des Kameraverbindungsproblems.](../../en/images/d22-04.png)

> Das ist ein Fehler, den nicht einmal Doubao lösen kann; das liegt alles daran, dass die lerobot-Bibliothek zu tief verschachtelt ist, und es ist für Anfänger sehr schwer zu debuggen

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## Mehrere Kameras anschließen: Teleoperation mit angezeigten Kamerabildern

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
