[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | Français | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# Référence de la ligne de commande

## Notes sur la ligne de commande

Avec visualisation en direct : --display_data=true

Sans visualisation en direct : --display_data=false

Avec `--display_data=true`, l'interface de visualisation très sympa de rerun.io est lancée, mais dans le répertoire `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` une image est enregistrée à chaque frame, ce qui prend beaucoup de place. Vous pouvez ensuite la régler sur `--display_data=false`.



Inférer un modèle depuis un dépôt de modèle HuggingFace : --policy.path=Tommymy/lerobot_my_model_a



## En prenant comme exemple la tâche Attraper des oranges

- Inférer un modèle local (avec visualisation en direct)

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

- Inférer un modèle local (sans visualisation en direct)

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

- Inférer un modèle depuis un dépôt de modèle HuggingFace

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

Le modèle est téléchargé après l'exécution

![Cette image montre l'interface d'exécution du script `pretrained_model.py` en ligne de commande. En haut, elle affiche la configuration des paramètres du modèle, tels que `--display_data=true` et `--policy.path=TommyZihao/lerobot_zihao_model_a`. En dessous figurent les réglages de paramètres comme « robot », « camera » et « calibration_dir ». En bas, elle montre la progression du téléchargement du modèle, actuellement à 68 %. L'image se rapporte à la description de l'exécution du script `pretrained_model.py` et de ses paramètres, présentant visuellement les réglages de paramètres et la progression du téléchargement.](../../en/images/d48-01.png)
