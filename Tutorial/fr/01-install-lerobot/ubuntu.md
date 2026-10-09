[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | Français | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Ordinateur Ubuntu

Le bras Leader noir utilise un adaptateur secteur 5V6A.

Le bras Follower blanc utilise un adaptateur secteur 12V5A.

## Installer Miniconda

https://www.anaconda.com/download

## Modifier le miroir pip

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## Modifier le miroir conda

```Shell
# Vider la configuration .condarc existante (facultatif, pour éviter les conflits)
echo "" > ~/.condarc

# Écrire la configuration du miroir de Tsinghua
cat << EOF > ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
EOF

# Vider le cache pour que la configuration prenne effet
conda clean -i
```

## Créer un environnement virtuel

```Shell
conda create -y -n lerobot python=3.12 -y
```

## Activer l'environnement virtuel

```Shell
conda activate lerobot
```

## Installer ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

Vérifiez que l'installation a réussi

```Shell
ffmpeg
```

<grid>
<column width-ratio="0.568354">
![L'image montre le résultat de l'activation d'un environnement virtuel conda et de l'installation de ffmpeg sur un ordinateur Ubuntu. D'abord, conda active l'environnement virtuel nommé lerobot, puis la commande conda install ffmpeg=7.1.1 -c conda -forge est exécutée, affichant les informations de Channels de conda, dont conda - forge, et enfin la Platform de linux - 64 avec les opérations completed Collecting package metadata et Solving environment. Cette image correspond à la section « Installer ffmpeg » et illustre l'exécution de l'installation.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![Cette image est une capture d'écran du terminal du système Ubuntu, montrant le résultat renvoyé après l'exécution de la commande ffmpeg. Elle affiche la version ffmpeg 7.1.1, ses informations de configuration et les numéros de version des modules pris en charge (comme libavcodec et libavformat), ainsi que des notes d'utilisation du convertisseur multimédia universel. Elle correspond à l'étape de vérification après l'installation de ffmpeg, qui sert à confirmer que l'outil ffmpeg a été installé avec succès sur le système.](../../en/images/d03-02.png)
</column>
</grid>

## Télécharger le dépôt officiel LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Installer le dépôt de code

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![Cette image montre le processus de saisie du répertoire lerobot dans le terminal du système Ubuntu et l'exécution de la commande `pip install -e.\[feetech\]`, qui fait partie de l'installation du dépôt officiel LeRobot. Elle montre clairement chaque étape de l'exécution de la commande, notamment la récupération des paquets depuis le dépôt Huawei Cloud indiqué, l'installation des dépendances et le téléchargement de paquets de jeux de données associés (comme diffusers, huggingface-hub et accelerate), où plusieurs paquets sont marqués avec leur progression de téléchargement, leur taille et leur vitesse spécifiques, pour se terminer par un message indiquant que les dépendances sont déjà satisfaites et que l'installation du dépôt est terminée.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![Cette image montre l'interface en ligne de commande du terminal du système Ubuntu lors d'une installation de paquets logiciels, avec les informations de gestion des dépendances obtenues lors de l'installation de logiciels tels que ffmpeg. Le terminal affiche la liste des paquets en cours de traitement, comme pytz, pyyaml et numpy, ainsi que le processus de désinstallation des versions existantes des paquets et d'installation des nouvelles, avec des notes sur la cohérence des dépendances. Ce contenu correspond à l'étape « Vérifier l'installation » qui suit « Installer ffmpeg », et constitue un enregistrement de la sortie du terminal lors de la vérification du processus d'installation de ffmpeg et d'autres logiciels.](../../en/images/d03-03.png)
</column>
</grid>

## Vérifier l'installation

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## Résultats sur une machine 4090

![L'image montre les commandes et les informations de LeRobot exécutées sur un ordinateur Ubuntu. La commande « Lerobot lerobot -info » affiche LeRobot version 0.4.3, la plateforme Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, la version de Python 3.12.0 et d'autres informations. On y trouve la version de PyTorch 2.7.1 + cu126, la version de CUDA 12.6 et le modèle de GPU NVIDIA GeForce RTX 4090. Cette image concerne la vérification d'une installation réussie et montre les informations de fonctionnement de LeRobot dans l'environnement Ubuntu.](../../en/images/d03-04.png)

![L'image montre l'interface interactive Python lors de l'exécution du dépôt LeRobot sur un ordinateur Ubuntu. Elle affiche la version Python 3.10.12, avec les informations d'empaquetage conda-forge et l'heure de compilation. L'utilisateur a saisi successivement les commandes `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()` et `import scservo_sdk`, obtenant le numéro de version de LeRobot 0.4.3, la disponibilité de CUDA True et l'import réussi de `scservo_sdk`. Cette image concerne la vérification de l'installation du dépôt LeRobot et illustre le processus de vérification.](../../en/images/d03-05.png)

## Résultats sur un NVIDIA DGX Spark

![L'image montre la sortie du terminal lors de l'exécution du dépôt LeRobot sur un ordinateur Ubuntu. Elle affiche LeRobot version 0.4.4, la version de CUDA 13.0 et le modèle de GPU NVIDIA GeForce GTX 1660 Ti. Elle liste également les versions de bibliothèques telles que HuggingFace Hub, Datasets et PyTorch, ainsi que les versions des outils FFmpeg et PyTorch. Enfin, elle vérifie les imports de LeRobot et torch ; torch.cuda.is_available() renvoie True, ce qui indique que CUDA est disponible. Cette image concerne l'exécution du dépôt LeRobot sur un ordinateur Ubuntu et montre les résultats.](../../en/images/d03-06.png)
