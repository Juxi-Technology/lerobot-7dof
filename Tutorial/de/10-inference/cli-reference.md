[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | Deutsch | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# Kommandozeilen-Referenz

## Hinweise zur Kommandozeile

Mit Live-Visualisierung: --display_data=true

Ohne Live-Visualisierung: --display_data=false

Mit `--display_data=true` wird die coole rerun.io-Visualisierungsschnittstelle gestartet, aber im Verzeichnis `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` wird für jedes Frame ein Bild gespeichert, was viel Platz beansprucht. Sie können es später auf `--display_data=false` setzen.



Ein Modell aus einem HuggingFace-Modell-Repo inferieren: --policy.path=Tommymy/lerobot_my_model_a



## Am Beispiel der Aufgabe „Grab Oranges"

- Ein lokales Modell inferieren (mit Live-Visualisierung)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Ein lokales Modell inferieren (ohne Live-Visualisierung)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Ein Modell aus einem HuggingFace-Modell-Repo inferieren

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --policy.path=Tommymy/lerobot_my_model_a
```

Das Modell wird nach dem Start heruntergeladen

![Dieses Bild zeigt die Oberfläche zum Ausführen des Skripts `pretrained_model.py` über die Kommandozeile. Oben ist die Konfiguration der Modellparameter zu sehen, etwa `--display_data=true` und `--policy.path=TommyZihao/lerobot_zihao_model_a`. Darunter folgen Parametereinstellungen wie „robot", „camera" und „calibration_dir". Unten ist der Fortschritt des Modell-Downloads zu sehen, derzeit bei 68 %. Das Bild bezieht sich auf die Beschreibung des Ausführens des Skripts `pretrained_model.py` und seiner Parameter und veranschaulicht die Parametereinstellungen und den Download-Fortschritt.](../../en/images/d48-01.png)
