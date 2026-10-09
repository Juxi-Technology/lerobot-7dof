[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | Français | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# Visualiser et rejouer un jeu de données

## Visualiser l'ensemble du jeu de données

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

Saisissez `TommyZihao/lerobot_zihao_dataset_a`, ou un autre jeu de données

![L'image montre l'interface du LeRobot Dataset Visualizer, avec un robot sur l'image et les mots « LeRobot Dataset Visualizer » en haut. Au milieu se trouve un menu déroulant affichant des options de jeux de données telles que « TommyZihao/lerobot_zihao_dataset_a », accompagné de « Example Datasets » et des noms de jeux de données en dessous, et d'un bouton bleu « Explore Open Datasets » plus bas. Cette image concerne la visualisation de l'ensemble du jeu de données mentionnée ci-dessus et correspond à l'opération de saisie du jeu de données indiqué.](../../en/images/d29-01.png)

![Image montrant addCriterion addCriterion](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![L'image montre l'interface de visualisation du jeu de données LeRobot de saisie d'orange. En haut se trouve une vidéo de la saisie d'une orange, l'orange étant maintenue contre un objet blanc. En dessous figurent des graphiques de données montrant les courbes de plusieurs variables au fil du temps, telles que « actuator », « gripper » et « gripper_pos ». À gauche se trouve une liste d'instructions, avec « Grab Orangesanges » actuellement sélectionné. Les boutons de lecture et de pause sont en bas à droite. Cette image concerne la visualisation d'un épisode précis et présente visuellement l'action de saisie et les données correspondantes.](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![L'image montre l'interface de visualisation d'un jeu de données LeRobot. À gauche se trouve une chronologie que l'on peut faire glisser pour consulter l'image à différents moments, et au milieu se trouve le flux vidéo de la caméra montrant deux mains en mouvement.](../../en/images/d29-04.png)
</column>
</grid>

Observation : la commande et l'état ne sont pas identiques — la commande est fournie par le bras Leader, et l'état est fourni par le bras Follower

## Visualiser un épisode précis

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![L'image montre l'interface de visualisation d'un épisode précis sur la plateforme rerun.io. À gauche se trouve la structure du jeu de données, affichant des données telles que observation_images. En haut au milieu se trouve le flux vidéo de la caméra en direct, avec une orange sur l'image. À droite figurent des courbes de données montrant comment différentes données évoluent au fil du temps. En bas se trouve une chronologie que l'on peut faire glisser pour consulter les données à n'importe quel moment. Cette image correspond à « Visualiser un épisode précis » et présente visuellement l'interface et les données lors de la consultation d'un épisode précis.](../../en/images/d29-05.png)

Faites glisser la chronologie pour consulter le flux vidéo et les positions des servomoteurs à n'importe quel moment

## Rejouer le mouvement du bras Follower pour un épisode précis

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

Vous entendrez `Replaying episode`, puis le bras Follower bouge, rejouant et reproduisant le mouvement de l'épisode indiqué

En fait, à ce stade, vous pouvez déjà impressionner beaucoup de profanes, n'est-ce pas

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
