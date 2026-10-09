[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | Français | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# Entraînement local sous Ubuntu

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- Remarque

Un `\` ne peut avoir qu'une seule espace avant lui, et aucune espace après

`--dataset.split` vaut `train` par défaut, ce qui signifie que l'intégralité du jeu de données est utilisée comme ensemble d'entraînement

Le jeu de données est local, donc `--dataset.streaming` doit valoir `false`, car les données sont déjà sur le disque et aucune lecture en flux n'est nécessaire

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![Cette image montre la commande d'entraînement et la configuration pour exécuter le script lerobot_train.py dans un environnement Ubuntu local. La commande inclut des paramètres tels que le chemin du jeu de données, l'ID de dépôt et la branche, par exemple `--dataset.repo_id` défini sur Tommy/lerobot_zhao_dataset_a. Parmi les valeurs de configuration, `--dataset.streaming` est défini sur `false`, `--use_imagenet_stats` sur `True`, `--batch_size` sur 4 et `--val_n_episodes` sur 1000. L'image est étroitement liée au contexte, présentant visuellement la commande d'entraînement et ses paramètres de configuration clés.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![Cette image montre le journal d'entraînement produit par l'exécution du script lerobot_train.py dans un environnement Ubuntu local, centré sur les paramètres de configuration de l'entraînement et l'état en direct de la session d'entraînement. Elle met clairement en évidence des informations d'entraînement clés : `--dataset.split` vaut `train` par défaut, ce qui signifie que l'intégralité du jeu de données est utilisée comme ensemble d'entraînement, et comme le jeu de données est stocké localement, l'état de `--dataset.streaming` est lui aussi fixé. Le journal couvre également la progression de l'entraînement, le chargement du jeu de données, la création de l'optimiseur et du planificateur du modèle, ainsi que les valeurs de loss et de step pendant l'entraînement, offrant une vue claire d'une session d'entraînement locale en cours.](../../en/images/d34-02.png)
</column>
</grid>

![Cette image montre le journal d'entraînement produit lors de l'entraînement d'un modèle avec LeRobot dans un environnement Ubuntu. Le journal enregistre les informations de la session d'entraînement, telles que l'heure, l'ensemble d'entraînement, le modèle, la loss et la précision. Les horodatages vont de 15:11:53 à 16:15:16 le 14 janvier 2024, la loss fluctue entre 0.68 et 0.65, et la précision (acc) entre 0.85 et 0.88. L'image se rapporte au contexte d'entraînement du modèle LeRobot, présentant visuellement l'évolution des métriques clés pendant l'entraînement.](../../en/images/d34-03.png)
