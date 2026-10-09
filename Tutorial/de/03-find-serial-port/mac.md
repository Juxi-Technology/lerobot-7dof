[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | Deutsch | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Mac-Rechner

## Den Port prüfen

```Shell
ls /dev/tty.*
```

Das Ergebnis sieht ähnlich aus wie im Bild unten; jeder der beiden Ports funktioniert

![Das Bild zeigt die Ausgabe im Mac-Terminal nach Ausführung des Befehls „ls /dev/tty.*", der mehrere Ports auflistet. Zwei davon, „/dev/tty.usbmodem5AAF2194741" und „/dev/tty.wchusbserial5AAF2194741", sind rot umrandet. Im Kontext geht es um das Prüfen der Ports, und das Ergebnis ähnelt diesem Bild; jeder der beiden Ports kann verwendet werden. Das Bild veranschaulicht die zu prüfenden Portinformationen, steht in engem Zusammenhang mit dem obigen Inhalt „Den Port prüfen" und zeigt das Ergebnis des Port-Prüfvorgangs.](../../en/images/d09-01.png)

## Berechtigungen für den Port erteilen

Geben Sie allen Benutzern Lese- und Schreibberechtigung für diese seriellen Geräte

```Shell
chmod 666 /dev/tty.*
```

## Meine Ports notieren

Follower-Arm:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader-Arm:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Warum gibt es auf einem Mac zwei Ports?

Das von uns verwendete Servo-Controller-Board wird **von macOS gleichzeitig als zwei verschiedene Arten von seriellen Treibern erkannt**, deshalb werden zwei Ports angezeigt:

- Einer ist der standardmäßige generische serielle Treiber des Systems (`/dev/tty.usbmodemxxxx`)
- Der andere ist der dedizierte serielle Treiber des Chip-Herstellers (zum Beispiel bezieht sich „wch" hier auf den CH340/CH341-Chip von Nanjing Qinheng) (`/dev/tty.wchusbserialxxxx`)

Das ist normal — **die beiden Ports entsprechen tatsächlich ein und demselben Hardwaregerät**, und Sie können über jeden der beiden eine Verbindung herstellen und kommunizieren (wählen Sie zum Beispiel in der Software, die den Roboterarm steuert, einfach einen der Ports).

Wenn beim späteren Verwenden eines Ports ein Fehler auftritt, versuchen Sie, auf den anderen zu wechseln.
