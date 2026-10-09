[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | Français | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Ordinateur Windows

Le bras Leader noir utilise un adaptateur secteur 5V6A.

Le bras Follower blanc utilise un adaptateur secteur 12V5A.

## Installer Miniconda

anaconda.com/download/success

Ou cliquez sur ce lien pour télécharger directement l'installateur

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![Cette image est l'écran d'installation de Miniconda3 sous Windows, montrant la version logicielle py312_24.7.1-0 (64-bit). L'écran propose deux options de type d'installation, dans lesquelles l'option intitulée « Just Me (recommended) » est mise en évidence par un cadre rouge et correspond à la méthode d'installation recommandée actuellement sélectionnée, tandis que l'autre option, « All Users (requires admin privileges) », n'est pas sélectionnée. Le haut de l'écran vous invite à choisir un type d'installation pour Miniconda3, et le bas comporte trois boutons : « Back », « Next » et « Cancel ». Cet écran est l'étape clé du flux d'installation de Miniconda pour confirmer la portée de l'installation.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![L'image montre les options d'installation avancées de l'écran d'installation de Miniconda3. L'option « Add Miniconda3 to my PATH environment variable » est mise en évidence par un cadre rouge, avec à côté une note expliquant que ce n'est pas recommandé car cela peut entrer en conflit avec d'autres applications, et suggérant plutôt les menus Invite de commandes et PowerShell ajoutés au menu Démarrer de Windows. Cette image concerne l'étape de création d'un environnement virtuel après « Modifier le miroir conda » et constitue une référence de configuration lors de l'installation de Miniconda.](../../en/images/d05-02.png)
</column>
</grid>

## Modifier le miroir conda

```Shell
# Effacer d'abord la configuration de miroir existante (pour éviter les conflits)
conda config --remove-key channels

# Remplacer les canaux par défaut de conda et les canaux tiers courants par le miroir de Tsinghua
# Ajouter les canaux de paquets par défaut (main/r/msys2)
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# Ajouter les canaux tiers courants
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# Activer l'affichage de la source de téléchargement, afin que l'adresse exacte soit affichée lors de l'installation des paquets
conda config --set show_channel_urls yes

# Vider le cache d'index pour que les nouveaux miroirs prennent effet
conda clean -i

# Afficher la configuration actuelle (pour vérifier que les canaux ont bien été ajoutés)
conda config --show-sources
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

![Cette image est la fenêtre de ligne de commande Windows, montrant le résultat de la vérification après l'exécution de la commande ffmpeg. Plus précisément, la ligne de commande affiche ffmpeg version 7.1.1, avec les informations de droits d'auteur et de compilation, ainsi que des informations sur les fichiers de bibliothèque associés à ffmpeg, et des notes d'utilisation en bas couvrant l'utilisation de base et la façon d'obtenir plus d'aide. Cette image sert à vérifier que ffmpeg a été installé avec succès sur un ordinateur Windows ; elle correspond à l'étape de vérification après « Installer ffmpeg » et illustre l'état d'exécution une fois l'installation de ffmpeg terminée.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![Il s'agit de l'interface du terminal Linux, montrant des commandes liées à conda et leur exécution. Deux commandes principales sont clairement identifiées : la commande d'activation de l'environnement virtuel nommé lerobot, « $ conda activate lerobot », et la commande de désactivation de l'environnement actif, « $ conda deactivate ». L'environnement (base) est actuellement activé, et le terminal exécute l'installation de ffmpeg 7.1.1 depuis le canal conda-forge, affichant plusieurs adresses de miroir configurées, tandis que le processus de collecte des métadonnées des paquets et de résolution de l'environnement de dépendances est déjà terminé.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![L'image montre l'interface du terminal utilisant la commande ffmpeg sous Ubuntu. Elle affiche les informations de version de ffmpeg, notamment le numéro de version, le constructeur et les détails de configuration du compilateur. Elle liste aussi les versions de divers codecs, comme libavcodec et libavformat. Des notes d'utilisation figurent en bas, vous invitant à utiliser « -h » pour toute l'aide, ou à exécuter « man ffmpeg ». Cette image concerne la section « Installer ffmpeg » et sert à vérifier une installation réussie de ffmpeg en affichant ses informations de version et de compilation.](../../en/images/d05-05.png)
</column>
</grid>

## Télécharger LeRobot

- Télécharger le dépôt officiel LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Installer le dépôt de code

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![L'image montre le résultat de la vérification dans la ligne de commande cmd de Windows après l'installation du dépôt de code LeRobot. La ligne de commande affiche des messages tels que « Successfully built lerobot », indiquant que l'installation a réussi. Elle liste également plusieurs paquets Python et leurs numéros de version, comme numpy 1.22.3 et scipy 1.7.1. Le bas affiche l'invite « (lerobot) C:\\Users\\40743\\Downloads\\lerobot> », indiquant que le répertoire actuel est le dossier lerobot dans Downloads. Cette image correspond à la section « Vérifier l'installation » et présente la rétroaction de la ligne de commande après une installation réussie.](../../en/images/d05-06.png)

## Vérifier l'installation

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![L'image montre l'écran de vérification d'une installation réussie dans l'environnement Python sous Windows. La ligne de commande affiche la version Python 3.10.19, a exécuté du code important des modules tels que lerobot, scservo_sdk et torch, et a finalement exécuté torch.cuda.is_available(), qui a renvoyé False. Cette image correspond à la section « Vérifier l'installation » et présente l'opération et le résultat de la vérification d'une installation réussie via l'environnement Python après l'installation du dépôt de code LeRobot.](../../en/images/d05-07.png)
