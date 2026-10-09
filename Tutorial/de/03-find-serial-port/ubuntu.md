[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | Deutsch | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# Methode 1: Direkt in der Linux-Kommandozeile prüfen

## Die seriellen Geräteports prüfen

```Shell
ls /dev/ttyACM*
```

## Die USB-Ports von Computer und Roboterarm verbinden

Schließen Sie zuerst den Follower-Arm an, dann den Leader-Arm

![Das Bild zeigt den Vorgang des Prüfens serieller Geräteports mit der Linux-Kommandozeile unter Ubuntu. Zuerst wird „nichts angeschlossen" angezeigt, dann erscheint nach dem Anschließen des Follower-Arms „/dev/ttyACM0"; anschließend erscheint nach dem Anschließen des Leader-Arms „/dev/ttyACM1". Das Bild steht in engem Zusammenhang mit dem Kontext und veranschaulicht, wie die seriellen Geräteports von der Kommandozeile aus sichtbar sind, nachdem Computer und USB-Ports des Roboterarms verbunden wurden, und hilft zu erklären, wie serielle Geräteports über die Linux-Kommandozeile geprüft werden.](../../en/images/d08-01.png)

# Methode 2: Das offizielle LeRobot-Werkzeug

```Shell
lerobot-find-port
```

![Das Bild zeigt die Kommandozeilenoberfläche zum Prüfen serieller Geräteports mit dem offiziellen LeRobot-Werkzeug unter Ubuntu. Der Befehl lautet „lerobot-find-port" und zeigt alle verfügbaren Ports an, darunter mehrere Ports „/dev/ttyACM". Die Aufforderung bittet Sie, das USB-Kabel des Follower abzuziehen und dessen Portnummer „/dev/ttyACM0" zu ermitteln, und das USB-Kabel anschließend wieder anzuschließen. Dieses Bild gehört zu Methode zwei zum Prüfen serieller Geräteports und veranschaulicht die Schritte und Ergebnisse.](../../en/images/d08-02.png)

![Das Bild zeigt den Vorgang der Verwendung des Befehls `lerobot-find-port` zum Prüfen der seriellen Geräteports des Roboterarms unter Ubuntu. Nach der Ausführung des Befehls werden alle verfügbaren Ports aufgelistet, dann werden Sie aufgefordert, das USB-Kabel des Leader abzuziehen, und schließlich wird `/dev/ttyACM1` als serielle Geräteportnummer des Leader-Arms angezeigt. Dieses Bild gehört zu Methode zwei zum Prüfen serieller Geräteports und veranschaulicht das Ergebnis der Ermittlung der Portnummern mit dem offiziellen LeRobot-Werkzeug.](../../en/images/d08-03.png)

# Meine Ports notieren

`/dev/ttyACM0` ist die serielle Geräteportnummer des Follower-Arms

`/dev/ttyACM1` ist die serielle Geräteportnummer des Leader-Arms

# Berechtigungen für den Port erteilen

Geben Sie allen Benutzern Lese- und Schreibberechtigung für diese seriellen Geräte

```Shell
sudo chmod 666 /dev/ttyACM*
```
