[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | Deutsch | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# Trainings-Kommandozeile – ACT (empfohlen für Einsteiger)

## Referenzdokumentation

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## Warum mit dem ACT-Algorithmus beginnen

ACT ist das am meisten empfohlene erste Modell zum Trainieren, wenn Sie in LeRobot einsteigen. Seine Vorteile sind:

- Das Modell ist sehr leichtgewichtig, mit nur 80 Millionen lernbaren Parametern
- Das Training konvergiert schnell, und auch die Inferenz ist schnell
- Sie sehen Ergebnisse bereits nach einer Stunde Training auf einer einzelnen GPU
- Das ACT-Modellarchiv ist etwa 200 MB groß und lässt sich leicht speichern und übertragen
- Das Sammeln von etwa 30 Episoden an Daten ist meist ausreichend
- Es lässt sich für die Inferenz auf einem Ubuntu-Host, einem Mac, einem Windows-PC, sogar einem Raspberry Pi einsetzen
- Die Inferenz auf einem realen Roboter funktioniert recht gut und ist für einfache Aufgaben wie Greifen, Händeschütteln und Ablegen eines Stifts mehr als ausreichend
- Der ACT-Algorithmus ist bereits in LeRobots Basisumgebung integriert, sodass keine zusätzlichen Bibliotheken nötig sind

## Kommandozeile

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## Hinweise zur Kommandozeile

Ein Zeilenumbruchzeichen `\` darf nur ein Leerzeichen davor und kein Leerzeichen danach haben

Rot dargestellte Parameter müssen vor jedem Lauf geprüft oder geändert werden

| Kommandozeilenparameter | Beschreibung |
|-|-|
| --dataset.repo_id | Repo_ID des HuggingFace-Datensatzes |
| --dataset.root | Lokaler Pfad zum Datensatz |
| --dataset.revision | Datensatzversion, angegeben beim Hochladen des Datensatzes zu HuggingFace |
| --dataset.streaming | Der Datensatz ist lokal, daher muss dieser Wert `false` sein, da die Daten bereits auf der Festplatte liegen und kein Streaming-Lesen nötig ist |
| --dataset.split | Standardmäßig `train`, das heißt, der gesamte Datensatz wird als Trainingssatz verwendet |
| --policy.type | Der zu trainierende Algorithmus, etwa act, smolvla, diffusion, pi0, wallx |
| --output_dir | Verzeichnis, in dem die Ausgabestruktur gespeichert wird |
| --job_name | Name dieses Trainingsjobs |
| --policy.device | Berechnungsgerät |
| --wandb.enable | wandb-Visualisierung aktivieren |
| --wandb.project | wandb-Projektname |
| --policy.push_to_hub | Das trainierte Modell zu HuggingFace pushen |
| --steps | Anzahl der Trainingsschritte |
| --batch_size | Datenmenge pro Schritt; reduzieren Sie diesen Wert, wenn Ihnen der GPU-Speicher ausgeht |
|  |  |

## Trainingsablauf

<grid>
<column width-ratio="0.357753">
![Dieses Bild zeigt ein Beispiel für die Verwendung des Trainingsbefehls `lerobot-train` von der Kommandozeile aus. Der Befehl legt mehrere Parameter fest, etwa `--dataset.repo_id` und `--dataset.root`, um Details des Datensatzes anzugeben, setzt `--policy.type` auf `act` und `--output_dir` auf das Ausgabeverzeichnis `outputs/lerobot_train/output_a`, zusammen mit weiteren Parametern wie `--job_name` und `--policy.device`. Außerdem listet er die Standardwerte von Parametern wie `--dataset.split` und `--policy.push_to_hub`. Das Bild steht in engem Bezug zum Kontext und zeigt, wie die Parameter des Trainingsbefehls gesetzt werden.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![Dieses Bild zeigt die Kommandozeilenausgabe während des Trainings. Es zeigt Trainingsdetails des Modells wie die Einstellungen für Scheduler, steps und use_policy_training_preset, zusammen mit datensatzbezogenen Parametern. Außerdem zeigt es Informationen wie die Anzahl der Modellparameter und den Loss, zum Beispiel num_total_params von 55917096 (52M) und einen Loss von 0,626. Darunter folgen Datei-Download-Informationen, etwa das Herunterladen von „https://download.pytorch.org/models/resnet18-f37072fd.pth" in das Verzeichnis /home/featurize/.cache/torch/hub/checkpoints. Das Bild bezieht sich auf die im Kontext beschriebene Trainings-Kommandozeile und zeigt, was die Kommandozeile während des Trainings ausgibt.](../../en/images/d36-02.png)
</column>
</grid>

![Dieses Bild zeigt die während des Trainings entstehenden Loginformationen. Das Log hält mehrere Trainingsschritte in chronologischer Reihenfolge fest, einschließlich Uhrzeit, Iterationszahl des Trainings, Loss und Lernrate – zum Beispiel betrug am 14. Januar 2024 um 15:11:53 die Iterationszahl 131k und der Loss 0,368. Dabei ist `INFO` der Logtyp, `train` die Trainingsphase, `step` die Iterationszahl, `loss` der Loss-Wert und `lr` die Lernrate. Das Bild bezieht sich auf den Abschnitt mit den Hinweisen zur Kommandozeile im Dokument und veranschaulicht die wichtigsten Daten des Trainingslaufs.](../../en/images/d36-03.png)

Das Modellarchiv ist etwa 300 MB groß
