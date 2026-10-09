[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | [Deutsch](../de/10-inference/infer-smolvla.md) | [Español](../es/10-inference/infer-smolvla.md) | Français | [Italiano](../it/10-inference/infer-smolvla.md) | [日本語](../ja/10-inference/infer-smolvla.md) | [한국어](../ko/10-inference/infer-smolvla.md) | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# Ligne de commande d'inférence - smolvla

## Ubuntu

- Supprimer le jeu de données existant préfixé par eval (le cas échéant)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Ligne de commande d'inférence

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

- Supprimer le jeu de données existant préfixé par eval (le cas échéant)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Ligne de commande d'inférence

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
![Cette image montre l'interface en ligne de commande d'inférence dans un environnement Ubuntu. En haut, elle montre la commande en cours d'exécution, incluant des réglages de paramètres tels que l'utilisation du cache et l'utilisation de Delta Joint Actions Aloha. En dessous figurent plusieurs informations clés, telles que l'identifiant du robot « zihao_follower_arm », une cible relative maximale de None, le port « /dev/tty.usbmodemSAAF2193661 » et une note indiquant que le nombre de couches VLM a été réduit à 16. L'image se rapporte à la ligne de commande d'inférence Ubuntu, présentant l'interface et une partie des réglages clés des paramètres.](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![Cette image montre le terminal pendant une session en ligne de commande d'inférence dans un environnement Ubuntu. En haut, elle montre la configuration liée au robot, comme calibration_dir et cameras. En dessous figurent les barres de progression de chargement de plusieurs fichiers json, tels que config.json et processor_config.json, indiquant le pourcentage de chargement et la taille. En bas se trouvent des messages de journal tels que « Mismatch between calibration values in the motor and the calibration file or no calibration file found », signalant une inadéquation entre les valeurs de calibration du moteur et le fichier de calibration. L'image correspond à la ligne de commande d'inférence Ubuntu, montrant le retour du terminal pendant l'opération.](../../en/images/d50-02.png)
</column>
</grid>

## Résultats

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
