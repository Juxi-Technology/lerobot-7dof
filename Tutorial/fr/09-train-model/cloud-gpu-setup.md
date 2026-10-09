[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | Français | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# Configurer un environnement d'entraînement sur GPU cloud

## Désactiver le proxy réseau de votre ordinateur

Sinon, vous ne pourrez peut-être pas ouvrir la ligne de commande Jupyter

## Se connecter à la plateforme de GPU cloud Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

Rejoignez le groupe d'utilisateurs, dites au service client que vous êtes un fan de Zihao de Tongji, et recevez un bon

## Démarrer une instance GPU cloud

## Installer et configurer l'environnement

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## Se connecter à wandb

```Shell
wandb login
Copiez-collez la clé API, puis appuyez sur Entrée
```

![Cette image montre le processus de connexion à W&B (Weights & Biases) à l'aide de la commande `wandb login` dans l'environnement LeRobot. Après l'exécution de la commande, W&B invite à se connecter à https://api.wandb.ai, indique qu'il est possible de déployer un serveur W&B local et guide pour trouver la clé API. Ensuite, W&B invite à coller la clé API et à appuyer sur Entrée, et signale également qu'aucun fichier .netrc n'a été trouvé et qu'il est en cours de création. Enfin, W&B affiche que vous êtes connecté en tant que tommyzihao, avec le statut de connexion https://api.wandb.ai. Cette image concerne l'étape « Se connecter à wandb » du document et présente visuellement le retour de l'opération de connexion.](../../en/images/d35-01.png)

## Monter le jeu de données

```Shell
Copiez la commande de téléchargement de l'instance, quelque chose comme :
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

Le jeu de données apparaît sous le répertoire `~`

## Ajuster la fréquence de sauvegarde des poids (facultatif)

Ouvrez `lerobot/src/lerobot/configs/train.py`

Passez save_freq de 20_000 à 5_000

Ainsi, vous obtenez les fichiers de poids du modèle plus tôt dans l'entraînement
