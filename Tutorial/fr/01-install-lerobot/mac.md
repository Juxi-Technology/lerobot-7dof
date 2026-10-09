[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | Français | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Ordinateur Mac

Le bras Leader noir utilise un adaptateur secteur 5V6A.

Le bras Follower blanc utilise un adaptateur secteur 12V5A.

## Accorder les autorisations

![Cette image montre une fenêtre des réglages système de macOS, actuellement sur la page des réglages d'Accessibilité, avec Confidentialité et sécurité sélectionné dans la barre latérale gauche. La fenêtre liste plusieurs applications, dont Baidu Netdisk, DingTalk et Doubao ; l'interrupteur de l'application Terminal est entouré en rouge et activé. Cela correspond à l'étape « Accorder les autorisations » du flux de travail sur Mac, qui active les autorisations Terminal nécessaires en préparation de l'installation de Miniconda et de la modification ultérieure des miroirs.](../../en/images/d04-01.png)

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
conda create -y -n lerobot python=3.12
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

![Cette image montre la sortie du terminal après l'exécution de la commande `ffmpeg`, utilisée pour vérifier que ffmpeg a bien été installé ; elle correspond à l'étape de vérification après « Installer ffmpeg ». La sortie affiche clairement FFmpeg version 7.1.1, les droits d'auteur des développeurs FFmpeg de 2000 à 2025, ainsi que les options de compilation, la liste des encodeurs pris en charge et les notes d'utilisation de l'Universal Media Converter, et se termine par une indication d'utiliser l'option `-h` ou la commande `man ffmpeg` pour plus d'aide.](../../en/images/d04-02.png)

## Télécharger LeRobot

- Télécharger le dépôt officiel LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Installer le dépôt de code

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![L'image montre l'installation du paquet « feetech » avec pip sur un Mac en ligne de commande. Elle affiche la progression de l'installation, notamment la récupération des paquets depuis « https://repo.huaweicloud.com/repository/pypi/simple/ » et le téléchargement de plusieurs fichiers tels que datasets, diffusers et huggingface-hub, pour se terminer par le téléchargement de « einops==0.8.0 ». Cette image se rapporte à la section « Installer le dépôt de code » et illustre l'exécution de la commande et son résultat lors de l'installation du dépôt.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![L'image montre l'écran de vérification sur un Mac après l'installation du dépôt de code LeRobot. Le terminal affiche « Successfully installed LeRobot » et liste plusieurs paquets Python installés avec leurs versions, comme numpy et pandas. Cette image correspond aux sections « Installer le dépôt de code » et « Vérifier l'installation » et présente les paquets installés afin que vous puissiez confirmer le succès de l'installation.](../../en/images/d04-04.png)
</column>
</grid>

## Vérifier l'installation

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![Cette image est une capture d'écran de la ligne de commande du terminal Mac, dans le cadre de la vérification de la configuration des étapes d'installation de LeRobot. Elle montre le répertoire de projet actuel lerobot-main ; après la commande python, l'environnement Python interactif s'ouvre avec la version Python 3.10.19 sous le système darwin. Les commandes import lerobot, import torch et torch.cuda.is_available() ont été exécutées successivement, et le résultat indique que la disponibilité de CUDA est False, suivi de import scservo_sdk. Cela correspond à l'étape de vérification, qui sert à confirmer l'état d'installation et de configuration de LeRobot et de ses dépendances.](../../en/images/d04-05.png)

![L'image montre la sortie du terminal d'une commande LeRobot sur un Mac. Elle affiche LeRobot version 0.4.3, la plateforme macOS - 15.6.1 - arm64 - arm - 64bit, la version de Python 3.12.12 et d'autres informations. Elle liste également les informations de version de Huggingface Hub, Datasets, NumPy, FFmpeg et PyTorch, si PyTorch est compilé avec le support CUDA, la version de CUDA et le modèle de GPU, et enfin la liste des scripts LeRobot. Cette image correspond au contexte de vérification et montre les informations de LeRobot après l'installation.](../../en/images/d04-06.png)
