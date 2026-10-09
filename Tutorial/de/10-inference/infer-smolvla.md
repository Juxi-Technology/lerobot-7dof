[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | Deutsch | [Español](../es/10-inference/infer-smolvla.md) | [Français](../fr/10-inference/infer-smolvla.md) | [Italiano](../it/10-inference/infer-smolvla.md) | [日本語](../ja/10-inference/infer-smolvla.md) | [한국어](../ko/10-inference/infer-smolvla.md) | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# Inferenz-Kommandozeile – smolvla

## Ubuntu

- Löschen Sie den vorhandenen, mit eval-präfixten Datensatz (falls vorhanden)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inferenz-Kommandozeile

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## Mac

- Löschen Sie den vorhandenen, mit eval-präfixten Datensatz (falls vorhanden)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inferenz-Kommandozeile

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/smolvla/40K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=2000
```

<grid>
<column width-ratio="0.425772">
![Dieses Bild zeigt die Inferenz-Kommandozeilenschnittstelle in einer Ubuntu-Umgebung. Oben ist der ausgeführte Befehl zu sehen, einschließlich Parametereinstellungen wie der Nutzung des Caches und der Nutzung von Delta Joint Actions Aloha. Darunter stehen mehrere wichtige Informationen, etwa die Roboter-ID „zihao_follower_arm", ein maximales relatives Ziel von None, der Port „/dev/tty.usbmodemSAAF2193661" und ein Hinweis, dass die Anzahl der VLM-Schichten auf 16 reduziert wurde. Das Bild bezieht sich auf die Ubuntu-Inferenz-Kommandozeile und zeigt die Oberfläche und einige der wichtigsten Parametereinstellungen.](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![Dieses Bild zeigt das Terminal während einer Inferenz-Kommandozeilensitzung in einer Ubuntu-Umgebung. Oben ist die roboterbezogene Konfiguration zu sehen, etwa calibration_dir und cameras. Darunter befinden sich die Ladefortschrittsbalken für mehrere json-Dateien wie config.json und processor_config.json, die Ladeprozent und Größe zeigen. Unten stehen Logmeldungen wie „Mismatch between calibration values in the motor and the calibration file or no calibration file found", die auf eine Abweichung zwischen den Motorkalibrierungswerten und der Kalibrierungsdatei hinweisen. Das Bild entspricht der Ubuntu-Inferenz-Kommandozeile und zeigt die Terminalrückmeldungen während des Betriebs.](../../en/images/d50-02.png)
</column>
</grid>

## Ergebnisse

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
