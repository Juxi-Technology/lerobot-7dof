[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | Français | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>Téléverser le jeu de données sur HuggingFace (facultatif)</title>

# Méthode 1 : téléverser en local (non recommandé ; vitesse de téléversement lente)

- Téléversement automatique

Définissez `push_to_hub=true` lors de la collecte du jeu de données, et il est téléversé automatiquement une fois la collecte terminée

![Cette image montre une interface en ligne de commande, une partie du journal d'exécution du processus de téléversement du jeu de données. En haut figurent les informations d'environnement pour des outils tels que SVN et treet W2 ; au milieu se trouve un message de traitement comme « Starting the second pass: moving the mov atom to the beginning of the file », et en dessous figurent des erreurs de l'exécution telles que « error messaging the mach port for IMCRunLoopWakeUpReliable », tandis qu'à droite sont listées la progression du traitement et des chiffres de transfert de données, tels que le volume de données et la vitesse pour différentes entrées. Dans l'ensemble, elle présente un enregistrement de l'état d'exécution pendant le traitement du téléversement du jeu de données.](../../en/images/d28-01.png)

- Téléversement manuel

Définissez `push_to_hub=false` lors de la collecte du jeu de données, et téléversez manuellement une fois la collecte terminée

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



Que ce soit en automatique ou en manuel, la vitesse de téléversement est très lente (environ 100 Ko par seconde)

car les serveurs de HuggingFace sont à l'étranger

# Méthode 2 : téléverser depuis une plateforme GPU cloud (recommandé)

## Se connecter à la plateforme GPU cloud Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## Démarrer une instance GPU cloud

## Téléverser l'archive du jeu de données dans `Datasets`

## Copier la commande de téléchargement de l'instance

![L'image montre la page datasets de la plateforme Featurize. En haut figure le titre « Datasets », et en dessous se trouve un jeu de données nommé « soarm_amazing_hand_pick.zip », de 213.3 MB, téléversé il y a 16 heures. À droite se trouvent un bouton « Cloud Unzip », ainsi que des boutons tels que « Like », « Comment » et « Copy Instance Download Command ». Cette image concerne l'étape « Téléverser l'archive du jeu de données dans `Datasets` » et montre la page datasets après le téléversement.](../../en/images/d28-02.png)

## Exécuter dans la ligne de commande de l'instance GPU cloud

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## Téléverser le jeu de données sur HuggingFace

Créez un fichier `upload_dataset.py` avec le contenu suivant

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

Exécutez le fichier

```Shell
python upload_dataset.py
```

![Cette image montre le processus d'exécution du téléversement du jeu de données dans la ligne de commande d'une instance GPU cloud, où un utilisateur nommé lerobot2 a exécuté la commande python upload.py. Elle affiche la progression du traitement des fichiers, avec 6 fichiers à traiter et tous à 100 % de progression, et indique la taille de transfert de chaque fichier, avec une progression totale du transfert de données à 100 %. En bas, elle précise qu'aucun fichier n'a été modifié depuis le dernier commit, de sorte que le commit est ignoré pour éviter de créer un commit vide. Ce contenu correspond à l'étape d'exécution du fichier upload_dataset.py.](../../en/images/d28-03.png)

- Une autre méthode de téléversement (non recommandée)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![Cette image montre le processus de téléversement d'un jeu de données avec la commande `hf upload` de HuggingFace, avec la commande `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`. L'image montre que le téléversement est entré dans sa phase finale, tous les fichiers étant à 100 % de progression, dont plusieurs fichiers vidéo et fichiers parquet dont les tailles de téléversement correspondent exactement aux tailles des fichiers locaux correspondants, ainsi que la taille totale des fichiers téléversés et la vitesse de transfert ; en bas se trouve un lien vers la page HuggingFace du jeu de données pour ce commit de téléversement, indiquant que la tâche de téléversement est terminée.](../../en/images/d28-04.png)

# Afficher le jeu de données sur HuggingFace

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![Cette image est une capture d'écran de la page de détails du jeu de données soarm_amazing_hand_pick de l'équipe Juxi-Technology sur la plateforme Hugging Face, correspondant au contenu « Afficher le jeu de données sur HuggingFace ». En haut figurent les options de navigation du jeu de données et des informations de base telles que l'auteur et les tags ; au milieu, la zone Dataset Viewer montre une partie des données d'entraînement du split 1 du jeu de données, notamment des champs tels que action, observation_state et timestamp avec leurs valeurs, et indique aussi la taille d'un enregistrement, le nombre total d'enregistrements et la taille totale. En bas, il est également mentionné des modèles associés entraînés sur ces données.](../../en/images/d28-05.png)

![L'image montre la page du jeu de données soarm_amazing_hand_pick sur la plateforme Hugging Face. En haut se trouvent une zone de recherche et une barre de navigation permettant de rechercher des modèles, des jeux de données, etc. La section des informations du jeu de données indique l'organisation propriétaire Juxi - Technology et des tags tels que robotics et imitation-learning. Sous l'onglet « Files and versions » sont listés des dossiers tels que data, meta et videos ainsi que le fichier README.md, avec l'indication de l'uploadeur, de la méthode de téléversement et de l'heure, par exemple « Upload README.md with huggingface_hub ». Cette image concerne la consultation d'un jeu de données HuggingFace et présente visuellement les fichiers et versions du jeu de données.](../../en/images/d28-06.png)
