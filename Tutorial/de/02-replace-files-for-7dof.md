[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | Deutsch | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# Schritt 2: Dateien ersetzen (für 7-DOF)

## 1. Wenn Sie das offizielle Code-Repository geklont haben: Welche Dateien müssen ersetzt/geändert werden

### Variante A: Den Code dieses Repositorys direkt verwenden (empfohlen)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Variante B: Nach dem Klonen des offiziellen lerobot manuell ersetzen

Kopieren Sie **3 Dateien** aus diesem Repository über Ihren offiziellen Clone:

| Datei dieses Repositorys (Quelle) | Überschreiben nach (Ziel) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | Die gleichnamige Datei im offiziellen Clone |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | Die gleichnamige Datei im offiziellen Clone |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> Voraussetzung: Ihr offizieller Clone hat dieselbe Struktur wie die Basis dieses Repositorys (das lerobot-Release 2026-09).
> 
> Wenn die Versionen stark abweichen, **überschreiben Sie nicht ganze Dateien**, sondern nehmen Sie die beiden „manuellen Änderungen" unten vor.

### Manuelle Änderung bei abweichenden Versionen (nur zwei Stellen)

**① Motor-Wörterbuch** (je eine Kopie in `so_follower.py` und `so_leader.py`, mit identischem Inhalt) — ändern Sie

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

zu

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # neu hinzugefügter Servo, Links-Rechts-Drehung
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # der ursprüngliche Rollservo Nr. 5, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② Kalibrierungslogik** (die `calibrate()` in beiden Dateien) — entfernen Sie den Sonderfall „Gelenk mit voller Umdrehung": ändern Sie

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

zu einer einzigen Zeile, die den tatsächlichen Bereich jedes Gelenks aufzeichnet:

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> Warum: Der Originalcode kodiert `wrist_roll` (Rotation um die Unterarmachse) fest als ein Gelenk, das sich ganz umdrehen kann (0\~4095), und setzt seinen Bereich auf den vollen Umfang. Nach der 7-DOF-Umstellung sind die beiden Handgelenke (yaw / roll, Servos 5/6) **beide mechanisch begrenzt und können sich nicht ganz umdrehen**; ein erzwungener Ganzumdrehungs-Bereich lässt den Code Gelenkbefehle an Winkel senden, die die Mechanik nicht erreichen kann, mit Beschädigungsrisiko. Jetzt wird bei der Kalibrierung für jeden Motor der tatsächliche Min-/Max-Wert manuell aufgezeichnet.

### Hinweis zu den zwei Follower-Armen

`bi_so_follower` / `bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) umhüllen lediglich einen einzelnen Arm und fügen ein Präfix `left_`/`right_` hinzu; sie **enthalten keine Motordefinitionen**. Sobald die **obigen Einarm-Dateien** erledigt sind, sind die Doppelarm-Befehle (`--robot.type=bi_so_follower`) automatisch 7-DOF.
