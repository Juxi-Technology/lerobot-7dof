[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | Deutsch | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# SO-ARM101 Dual-Arm-Tutorial

## Einführung

Diese Anleitung beschreibt den vollständigen Workflow zum Trainieren eines Dual-Arm-SO-ARM-Robotersystems mit LeRobot, einschließlich Hardware-Verkabelung, Dual-Arm-Kalibrierung, Dual-Arm-Teleoperation, Aufzeichnung und Verwaltung von Datensätzen, ACT-Policy-Training und Deployment auf dem realen Roboter. Mit dieser Anleitung können Sie zwei Leader-Arme und zwei Follower-Arme verwenden, um Demonstrationsdaten zu erfassen, eine Imitationslern-Policy zu trainieren und sie auf den realen Armen auszuführen.

Verdrahten Sie zunächst alles wie folgt

| Rolle | Port |
|-|-|
| Linker Follower | /dev/ttyACM0 |
| Rechter Follower | /dev/ttyACM1 |
| Linker Leader | /dev/ttyACM2 |
| Rechter Leader | /dev/ttyACM3 |

Der Follower-Typ ist so101_follower und der Leader-Typ ist so101_leader (in LeRobot teilen sich so100_leader und so101_leader dieselbe Implementierung).

## Voraussetzungen

### 0.1 Abhängigkeiten installieren

Informationen zur Einrichtung der Umgebung finden Sie im SO-ARM-Tutorial:

### 0.2 USB-Berechtigungen

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## Kalibrierung (kritischer Schritt)

### 1.1 Den linken Follower kalibrieren

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 Den rechten Follower kalibrieren

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 Den linken Leader kalibrieren

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 Den rechten Leader kalibrieren

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

Nach der Kalibrierung werden die Dateien gespeichert unter:

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> Hinweis zu den Verzeichnisnamen: so101_follower und so100_follower sowie so101_leader und so100_leader teilen sich dieselbe Implementierung, daher sind die Verzeichnisse als so_follower / so_leader vereinheitlicht. Der Leader ist ein Teleoperator, seine Kalibrierungsdateien liegen daher unter teleoperators/ statt unter robots/.

### (Optional) Wenn Sie zuvor mit anderen IDs kalibriert haben

Wenn Sie zuvor zum Beispiel my_awesome_follower_arm1, my_awesome_follower_arm2 usw. verwendet haben, können Sie die Kalibrierungsdateien kopieren:

```Bash
CAL_DIR=~/.cache/huggingface/lerobot/calibration

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm1.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_left.json

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm2.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_right.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm3.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_left.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm4.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_right.json
```

---

## Dual-Arm-Teleoperation

### 2.1 Ohne Kameras

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### 2.2 Mit Kameras

Sie können mit `lerobot-find-cameras opencv` die Kamera-Indizes prüfen und Kameras nach Belieben hinzufügen oder entfernen.

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### Sicherheitshinweise

- Achten Sie auf die Umgebung und vermeiden Sie Kollisionen zwischen den Follower-Armen.

## Einen Datensatz aufnehmen

### 3.1 Lokal speichern (ohne Upload zum Hub)

Fügen Sie `--dataset.root` (das Verzeichnis, in das die Daten geschrieben werden) und `--dataset.push_to_hub=false` hinzu und ergänzen Sie `--dataset.no_stamp=true`, um den Datensatznamen stabil zu halten (andernfalls wird automatisch ein Zeitstempel an die repo_id angehängt, und späteres Fortsetzen/Wiedergeben/Trainieren findet ihn nicht mehr).

> Hinweis: Die repo_id sollte ein / enthalten (in der Form Benutzername/datensatz-name); ein lokaler Datensatz wird nicht wirklich hochgeladen.

```Bash
lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> Die Video-Codierung ist standardmäßig bereits libsvtav1 und muss nicht angegeben werden; zur Anpassung verwenden Sie einen verschachtelten Parameter wie `--dataset.rgb_encoder.vcodec=h264`.

Die Daten werden unter ./datasets/bi_so101_task/ mit folgender Struktur gespeichert:

```Bash
├── meta/
│   ├── info.json         # Datensatz-Infos (fps, Feature-Formen usw.)
│   ├── episodes/         # Metadaten pro Episode (chunk-000/...)
│   ├── stats.json        # Normalisierungsstatistiken je Feature
│   └── tasks.parquet     # Aufgabentext → task_index
├── data/                 # Feature-Daten pro Frame (chunk-*.parquet)
└── videos/               # Ein Unterverzeichnis pro Kamera (chunk-*.mp4)
```

### 3.2 Upload zum Hugging Face Hub

Wenn Sie den automatischen Upload wünschen, behalten Sie HF_USER bei und entfernen Sie root und push_to_hub=false (Upload ist die Standardeinstellung). Halten Sie die Ports und Kamera-Indizes konsistent mit der Verkabelungstabelle:

```Bash
export HF_USER=your_hf_username

lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> Der Name des hochgeladenen Hub-Repositorys ist ${HF_USER}/bi_so101_task und stimmt mit der repo_id überein, die für das Hub-basierte Training in 4.2 unten verwendet wird. Eine lokale Kopie wird zunächst unter ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/ gespeichert.

### 3.3 Aufnahme fortsetzen (resume)

Wenn die Aufnahme unerwartet beendet wurde (zum Beispiel haben Sie in der Reset-Phase mit einem Rechtsklick beendet) oder Sie die Erfassung über mehrere Sitzungen verteilen möchten, verwenden Sie `--resume`, um weiterhin Episoden an denselben Datensatz anzuhängen.

**Hinweis**:

- Sie müssen `--resume=true` hinzufügen, sonst bricht `LeRobotDataset.create()` mit einem Fehler ab, weil das Verzeichnis bereits existiert.
- Im resume-Befehl müssen `--dataset.root` und `--dataset.repo_id` exakt mit der ersten Aufnahme (3.1) übereinstimmen (resume erfordert ein explizites root).
- `--dataset.num_episodes` ist **die Anzahl der diesmal aufzunehmenden Episoden**, nicht das Gesamtziel. Wenn Sie bereits 15 aufgezeichnet haben und insgesamt 50 möchten, schreiben Sie beispielsweise 35.
- Versuchen Sie beim Beenden, während einer Episodenaufzeichnung oder direkt nach deren natürlichem Ende zu beenden; vermeiden Sie das Beenden in der Phase „Reset the environment“ (sie führt dazu, dass eine leere Episode nicht gespeichert werden kann).

```Bash
lerobot-record \
  --resume=true \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=35 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

### 3.4 Wiedergabe und Löschen von Episoden

#### Eine bestimmte Episode wiedergeben

```Bash
lerobot-replay \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.episode=24
```

> episode ist ein 0-basierter Index, 24 bedeutet also die 25. Episode.

#### Eine bestimmte Episode löschen

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

Das Löschen schreibt den Datensatz an Ort und Stelle neu, und die Originaldaten werden nach ./datasets/bi_so101_task_old/ gesichert. Sobald Sie bestätigt haben, dass der neue Datensatz korrekt ist, können Sie das Backup manuell entfernen:

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### Den gesamten Datensatz löschen

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## ACT-Training

### 4.1 Aus einem lokalen Datensatz trainieren

```Bash
lerobot-train \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=60000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> `--dataset.root` verweist auf das in 3.1 aufgezeichnete Datensatzverzeichnis (die repo_id muss mit der bei der Aufnahme verwendeten übereinstimmen). Wenn das Verzeichnis `--output_dir` bereits existiert, wird sofort ein FileExistsError ausgelöst – verwenden Sie ein neues Ausgabeverzeichnis oder fügen Sie `--resume=true` hinzu, um das Training fortzusetzen.

### 4.2 Vom Hugging Face Hub trainieren

```Bash
export HF_USER=your_hf_username

lerobot-train \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=100000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> Der obige Befehl verwendet die Standardparameter von ACT (chunk_size=100, dim_model=512 usw.).
> 
> Die repo_id muss mit dem beim Upload in 3.2 verwendeten Repository-Namen übereinstimmen (3.2 fügt `--dataset.no_stamp=true` hinzu, daher ist der Repository-Name als \${HF_USER}/bi_so101_task festgelegt). Für das Training ist kein `--dataset.root` nötig; er wird automatisch vom Hub heruntergeladen.

## Deployment auf dem realen Roboter

> Hinweis: `lerobot-record` dient nur zum Sammeln von Demonstrationsdaten. Verwenden Sie `lerobot-rollout`, um eine trainierte Policy einzusetzen – die aktuelle Version von `lerobot-record` akzeptiert `--policy.path` nicht mehr und lehnt zudem Datensatznamen mit dem Präfix eval\_ ab.

### 5.1 Bewertung vor Ort (keine Datenerfassung)

```Bash
lerobot-rollout \
  --strategy.type=base \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --task="Pick the cube with left arm and hand it to right arm" \
  --duration=60 \
  --display_data=true
```

- `--duration` ist die Anzahl der Sekunden, die ausgeführt wird; 0 bedeutet kein Zeitlimit.
- Um mitten im Lauf zu übernehmen/abzubrechen, fügen Sie `--interactive=true` hinzu und verwenden Sie Befehle wie /stop und /reset im Terminal.

### 5.2 Bewerten und Daten aufzeichnen (lokal)

Verwenden Sie die episodic-Strategie (verhält sich wie das alte lerobot-record: zeichnet episodenweise mit einer Reset-Phase auf):

```Bash
lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=juxi/rollout_bi_so101_task \
  --dataset.root=./datasets/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

> Ein Deployment-Datensatzname muss mit rollout\_ beginnen (eine harte Anforderung der aktuellen Version). Fügen Sie bei lokaler Aufzeichnung `--dataset.root` und `--dataset.no_stamp=true` hinzu, damit kein Zeitstempel an den Verzeichnisnamen angehängt wird.

### 5.3 Bewertungsdaten zum Hugging Face Hub hochladen

```Bash
export HF_USER=your_hf_username

lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=${HF_USER}/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

## FAQ

| Problem | Ursache | Lösung |
|-|-|-|
| Die Teleoperation fordert eine Neukalibrierung | bi_so_follower findet keine Kalibrierungsdateien mit dem Suffix \_left / \_right | Kalibrieren Sie mit IDs neu, die _left / \_right enthalten, oder kopieren Sie vorhandene Kalibrierungsdateien |
| Der Leader-Arm lässt sich nicht ziehen | Das Drehmoment des Leaders ist nicht deaktiviert | Neu kalibrieren oder den Motor prüfen |
| Beim Fortsetzen der Aufnahme wird gemeldet, dass das Verzeichnis bereits existiert | `--resume=true` wurde nicht hinzugefügt | Fügen Sie `--resume=true` zum lerobot-record-Befehl hinzu |
| `--resume=true` bricht mit einem Fehler ab und verlangt ein root | Resume erfordert ein explizites Datensatzverzeichnis | Fügen Sie `--dataset.root=./datasets/bi_so101_task` zum resume-Befehl hinzu, passend zur ersten Aufnahme |
| Der Datensatzverzeichnisname enthält einen zusätzlichen Zeitstempel, sodass Wiedergabe/Training ihn nicht findet | no_stamp wurde bei der Aufnahme nicht gesetzt, daher wurde ein Zeitstempel an die repo_id angehängt | Fügen Sie beim Aufnehmen/Fortsetzen `--dataset.no_stamp=true` hinzu |
| `--dataset.vcodec=...` meldet, dass der Parameter nicht existiert | Es ist ein alter Parameter; der Video-Codierungsparameter ist jetzt verschachtelt | Verwenden Sie stattdessen `--dataset.rgb_encoder.vcodec=h264` (der Standard ist bereits libsvtav1) |
| Beim Deployment meldet lerobot-record einen `--policy.path` / eval\_-Fehler | Die aktuelle Version von lerobot-record enthält kein Policy-Deployment mehr | Verwenden Sie `lerobot-rollout --strategy.type=episodic` für das Deployment, mit Datensatznamen, die mit rollout_ beginnen |
| Die linken und rechten Arme sind vertauscht | Falsche Port-Konfiguration | Vertauschen Sie left_arm_config.port und right_arm_config.port |
| Das Training findet den Datensatz nicht | Für den lokalen Datensatz wurde kein root angegeben | Fügen Sie beim Training `--dataset.root=./datasets/xxx` hinzu |
| Der Datensatz wird automatisch hochgeladen | push_to_hub=false wurde nicht gesetzt | Fügen Sie beim Aufnehmen `--dataset.push_to_hub=false` hinzu |
| Beim Beenden erscheint die Meldung You must add one or several frames before calling add_episode | Sie haben in der Reset-Phase beendet, sodass die aktuelle Episode keine Frames enthält | Betrifft bereits aufgezeichnete Daten nicht; verwenden Sie `--resume=true`, um die Erfassung fortzusetzen |
