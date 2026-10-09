[English](../en/index.md) | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | Deutsch | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>SO-ARM101 7-DOF-Tutorial</title>

# SO-ARM101 7-DOF · Vorbereitung vor dem Betrieb

> Diese Anleitung richtet sich an alle, die **einen SO-ARM101 von 6 Servos auf 7 Servos umgebaut** haben und nun den vollständigen LeRobot-Ablauf (Kalibrierung → Aufnahme → Training → Deployment) damit durchführen möchten.  
> Zugehöriger Code: dieses Repository (`lerobot-7dof`), ein Fork des offiziellen lerobot, der nur die SO-bezogene Motorkonfiguration ändert.

---

## 0. Zuerst den Roboterarm prüfen

7 Servos (alle STS3215). Zuordnung von Servo-IDs zu Gelenken:

| **Servo-ID** | **Gelenkname** | **Beschreibung** |
|-|-|-|
| 1 | `shoulder_pan` | Horizontale Drehung der Schulter |
| 2 | `shoulder_lift` | Anheben der Schulter |
| 3 | `elbow_flex` | Beugung des Ellenbogens |
| 4 | `wrist_flex` | Neigung des Handgelenks (Auf- und Abbewegung) |
| 5 | `wrist_yaw` | Gierung des Handgelenks (Links-Rechts-Drehung um etwa 90°) · **der diesmal neu hinzugefügte Servo** (zwischen den ursprünglichen Nummern 4 und 5 eingefügt) |
| 6 | `wrist_roll` | Rollen des Handgelenks · ursprünglich der Rollservo Nr. 5, ID 5→6; das Druckteil bleibt unverändert, der Name bleibt gleich |
| 7 | `gripper` | Greifer · ursprünglich ID=6, nach der Änderung auf 7 verschoben |

Reihenfolge der Gelenkdaten (die Reihenfolge der Gelenkdimensionen von `action` / `observation.state` in der Parquet-Datei nach der Aufnahme):  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`.

⚠️ Hinweis: **Die Daten, Kalibrierungsdateien und bereits trainierten Modelle der 6-Servo-Version sind mit diesem Repository nicht kompatibel**; führen Sie daher alles im Folgenden erneut durch.

---

## 1. Wenn Sie das offizielle Code-Repository geklont haben: Welche Dateien müssen ersetzt/geändert werden

### Variante A: Den Code dieses Repositorys direkt verwenden (empfohlen)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Variante B: Nach dem Klonen des offiziellen lerobot manuell ersetzen

Kopieren Sie **3 Dateien** aus diesem Repository über Ihren offiziellen Clone:

| Datei dieses Repositorys (Quelle) | Überschreiben nach (Ziel) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | Die gleichnamige Datei im offiziellen Clone |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | Die gleichnamige Datei im offiziellen Clone |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | Die gleichnamige Datei im offiziellen Clone (**nur Kommentarkorrektur**, keine funktionale Auswirkung; optional) |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <offizieller-Clone>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <offizieller-Clone>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> Voraussetzung: Ihr offizieller Clone hat dieselbe Struktur wie die Basis dieses Repositorys (das lerobot-Release 2026-08). Wenn die Versionen stark abweichen, **überschreiben Sie nicht ganze Dateien**, sondern nehmen Sie die beiden „manuellen Änderungen" unten vor.

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
