[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | Français | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# Collecter un jeu de données par démonstration — Handshake 200

## Créer un dépôt de jeu de données sur HuggingFace

https://huggingface.co/new-dataset

![L'image montre l'interface de création d'un nouveau dépôt de jeu de données sur HuggingFace. Le champ « Owner » affiche TommyZihao, le champ « Dataset name » est rempli avec lerobot_zihao_dataset_shake200, la « License » est définie sur mit, et l'option « Public » est sélectionnée ; une note en dessous précise que tout le monde peut consulter ce jeu de données et que seuls le propriétaire du jeu de données ou les membres de l'organisation peuvent committer. En bas, il est indiqué qu'après avoir créé le jeu de données, vous pouvez téléverser des fichiers via l'interface web ou git, et un bouton « Create dataset » figure en bas. Cette image concerne le contenu sur la création d'un dépôt de jeu de données sur HuggingFace et montre l'interface de l'opération de création de jeu de données.](../../en/images/d27-01.png)

## Supprimer tout jeu de données existant portant le même nom (le cas échéant)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Collecte du jeu de données Shake200

Une caméra, collecte d'un jeu de données - ordinateur Mac

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
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake200 \
    --dataset.num_episodes=200 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```

## Pendant la collecte

<grid>
<column width-ratio="0.508765">
![Il s'agit de l'enregistrement d'écran du terminal pendant la collecte du jeu de données de poignée de main (Shake200). En haut et dans la partie médiane basse de l'interface figure l'information « INFO 202-04-15 17:00:22.227 Recording episode 36 », indiquant que le 36e épisode de collecte de données est en cours d'enregistrement. Elle présente aussi les paramètres pertinents de l'encodeur SVT-AV1 et l'information de progression « 200/200 \[00:00:00::00, 4911.09 examples/s\] », reflétant que cette étape se situe au milieu de la collecte du jeu de données, en correspondance avec l'étape « Pendant la collecte » décrite dans le document, et montrant l'état d'exécution du système au moment de la collecte.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![Cette image est l'interface d'exécution du terminal Mac, montrant la configuration et les journaux d'exécution du projet SVT. L'interface contient plusieurs paramètres de configuration, tels que la taille du GOP de la configuration SVT, la taille du mini-pod, le type de key-frame et les valeurs CRF, ainsi que les informations de version de la bibliothèque SVT-A1 Encoder. Les journaux de la moitié inférieure enregistrent des opérations sur les fichiers vidéo, mentionnant le déplacement de l'atome au début du fichier, ainsi que les états de connexion/déconnexion de plusieurs composants comme OpenVCamera et SV280 follower, et affichent enfin l'invite de fin d'exécution du terminal dans le chemin courant, correspondant au scénario d'exécution du terminal lors de la collecte par démonstration du jeu de données de poignée de main 200.](../../en/images/d27-02.png)
</column>
</grid>

Commandes par les touches fléchées du clavier :  
→ (Flèche droite) Terminer l'épisode en cours de manière anticipée ; passer à l'épisode suivant.  
← (Flèche gauche) Annuler l'épisode en cours ; l'enregistrer à nouveau.  
ESC, arrêter immédiatement, encoder la vidéo et téléverser le jeu de données.

## Collecte terminée — Répertoire d'enregistrement du jeu de données

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
