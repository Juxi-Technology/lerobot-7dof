[English](../en/10-inference/infer-wall-oss.md) | [简体中文](../zh-hans/10-inference/infer-wall-oss.md) | [繁體中文](../zh-hant/10-inference/infer-wall-oss.md) | Deutsch | [Español](../es/10-inference/infer-wall-oss.md) | [Français](../fr/10-inference/infer-wall-oss.md) | [Italiano](../it/10-inference/infer-wall-oss.md) | [日本語](../ja/10-inference/infer-wall-oss.md) | [한국어](../ko/10-inference/infer-wall-oss.md) | [Português (BR)](../pt-br/10-inference/infer-wall-oss.md) | [Português (PT)](../pt-pt/10-inference/infer-wall-oss.md)

# Inferenz-Kommandozeile – WALL-OSS

## Ubuntu

- Löschen Sie den vorhandenen, mit eval-präfixten Datensatz (falls vorhanden)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inferenz-Kommandozeile

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

![Dieses Bild zeigt das Terminal während einer Inferenz-Kommandozeilensitzung in einer Ubuntu-Umgebung. Es zeigt mehrere Parametereinstellungen, etwa eine Gesamtvideopixelzahl von 90316800 und eine Preprocessing-Konfigurationsdateigröße von 2,46 KB. Außerdem zeigt es den Ladefortschritt für Dateien wie tokenizer.json und tokenizer_config.json, zum Beispiel tokenizer.json mit 100 % geladen. Oben stehen „INFO"-Meldungen, unten Modellladehinweise wie „Loading model from:". Das Bild entspricht der Ubuntu-Inferenz-Kommandozeile und zeigt die wichtigsten Informationen während des Betriebs.](../../en/images/d53-01.png)

<figure view-type="Preview">[Attachment: c7b8a795e52ca10689d296d212dc8e53.mp4](../../en/images/c7b8a795e52ca10689d296d212dc8e53.mp4)</figure>





## Inferenz-Kommandozeile – Mac

- Löschen Sie den vorhandenen, mit eval-präfixten Datensatz (falls vorhanden)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inferenz-Kommandozeile

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

<figure view-type="Preview">[Attachment: 9f4567abdcce7f983415aef8f42877d0.mp4](../../en/images/9f4567abdcce7f983415aef8f42877d0.mp4)</figure>
