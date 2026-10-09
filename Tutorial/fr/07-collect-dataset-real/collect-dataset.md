[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | Français | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# Collecter un jeu de données par démonstration

## Supprimer tout jeu de données existant portant le même nom (le cas échéant)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## Une caméra, collecte d'un jeu de données - ordinateur Mac

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## Deux caméras, collecte d'un jeu de données - ordinateur Mac

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=true \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## Pendant la collecte

<grid>
<column width-ratio="0.508765">
![L'image montre l'interface du terminal pendant la collecte d'un jeu de données avec OpenVSLAM sur un Mac. En haut sont affichés les paramètres de collecte, tels que la résolution, la fréquence d'images et l'encodeur. En dessous se trouve le journal de collecte, enregistrant l'heure de début de collecte, les informations de version, le nombre de threads et l'encodeur, et montrant aussi la progression de la collecte, par exemple 298/298 épisodes collectés, 5119.33 secondes au total. En bas figurent des notes pour la touche « ESC », telles que l'arrêt immédiat et le téléversement du jeu de données. Cette image concerne le flux de travail de collecte de jeux de données et présente visuellement la rétroaction du terminal pendant la collecte.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![Cette image montre l'interface du terminal en ligne de commande sur un Mac, utilisée pour afficher les informations de journal d'exécution liées à la collecte d'un jeu de données par caméra. Elle contient des paramètres de configuration liés à SVT, tels que les paramètres de configuration, la version de la bibliothèque d'encodage et les valeurs de chaque élément de configuration (comme la key frame et le CRF, la résolution d'encodage), et montre aussi des journaux d'état d'exécution, tels que des messages sur le traitement de fichiers MP4, des enregistrements de déconnexion d'appareils et des horodatages pendant l'exécution du programme. Dans l'ensemble, elle présente l'état d'exécution en arrière-plan pendant la collecte d'un jeu de données par caméra.](../../en/images/d26-02.png)
</column>
</grid>

Commandes par les touches fléchées du clavier :  
→ (Flèche droite) Terminer l'épisode en cours de manière anticipée ; passer à l'épisode suivant.  
← (Flèche gauche) Annuler l'épisode en cours ; l'enregistrer à nouveau.  
ESC, arrêter immédiatement, encoder la vidéo et téléverser le jeu de données.

## Collecte terminée — Répertoire d'enregistrement du jeu de données

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```





## Poignée de main

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.num_episodes=30 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```
