[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | Italiano | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# Riferimento della riga di comando

## Note sulla riga di comando

Con visualizzazione in tempo reale: --display_data=true

Senza visualizzazione in tempo reale: --display_data=false

Con `--display_data=true` viene avviata la bella interfaccia di visualizzazione rerun.io, ma nella directory `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` viene salvata un'immagine per ogni fotogramma, il che occupa molto spazio. Puoi impostarla su `--display_data=false` in seguito.



Inferisci un modello da un repo di modelli HuggingFace: --policy.path=Tommymy/lerobot_my_model_a



## Esempio con il compito Grab Oranges

- Inferisci un modello locale (con visualizzazione in tempo reale)

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

- Inferisci un modello locale (senza visualizzazione in tempo reale)

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

- Inferisci un modello da un repo di modelli HuggingFace

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

Il modello viene scaricato dopo l'esecuzione

![Questa immagine mostra l'interfaccia per eseguire lo script `pretrained_model.py` dalla riga di comando. In alto mostra la configurazione dei parametri del modello, come `--display_data=true` e `--policy.path=TommyZihao/lerobot_zihao_model_a`. Sotto ci sono impostazioni di parametri come "robot", "camera" e "calibration_dir". In fondo mostra l'avanzamento del download del modello, attualmente al 68%. L'immagine è collegata alla descrizione dell'esecuzione dello script `pretrained_model.py` e dei suoi parametri e presenta visivamente le impostazioni dei parametri e l'avanzamento del download.](../../en/images/d48-01.png)
