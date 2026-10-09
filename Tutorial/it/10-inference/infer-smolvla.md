[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | [Deutsch](../de/10-inference/infer-smolvla.md) | [Español](../es/10-inference/infer-smolvla.md) | [Français](../fr/10-inference/infer-smolvla.md) | Italiano | [日本語](../ja/10-inference/infer-smolvla.md) | [한국어](../ko/10-inference/infer-smolvla.md) | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# Riga di comando di inferenza - smolvla

## Ubuntu

- Elimina il dataset esistente con prefisso eval (se presente)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Riga di comando di inferenza

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

- Elimina il dataset esistente con prefisso eval (se presente)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Riga di comando di inferenza

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
![Questa immagine mostra l'interfaccia a riga di comando di inferenza in un ambiente Ubuntu. In alto mostra il comando in esecuzione, con impostazioni di parametri come l'uso della cache e l'uso di Delta Joint Actions Aloha. Sotto ci sono diverse informazioni chiave, come l'id del robot "zihao_follower_arm", un massimo target relativo pari a None, la porta "/dev/tty.usbmodemSAAF2193661" e una nota che il numero di livelli del VLM è stato ridotto a 16. L'immagine è collegata alla riga di comando di inferenza su Ubuntu e presenta l'interfaccia e alcune impostazioni dei parametri principali.](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![Questa immagine mostra il terminale durante una sessione di inferenza da riga di comando in un ambiente Ubuntu. In alto mostra la configurazione relativa al robot, come calibration_dir e cameras. Sotto ci sono le barre di avanzamento del caricamento di diversi file json, come config.json e processor_config.json, che mostrano la percentuale di caricamento e la dimensione. In fondo ci sono messaggi di log come "Mismatch between calibration values in the motor and the calibration file or no calibration file found", che segnala una discrepanza tra i valori di calibrazione del motore e il file di calibrazione. L'immagine corrisponde alla riga di comando di inferenza su Ubuntu e mostra il feedback del terminale durante l'operazione.](../../en/images/d50-02.png)
</column>
</grid>

## Risultati

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
