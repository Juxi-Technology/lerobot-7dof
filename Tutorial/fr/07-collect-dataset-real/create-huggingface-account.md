[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | Français | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Créer un compte Hugging Face (facultatif)

## Configurer un miroir HuggingFace basé en Chine

- Ubuntu

```Shell
sudo nano ~/.bashrc

# Ajouter ceci à la fin du fichier
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# Sortie
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# Ajouter ceci à la fin du fichier
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# Sortie
# https://hf-mirror.com
```



## Créer un jeton

https://huggingface.co/settings/tokens

![L'image montre la page « Paramètres / Jetons » du site Hugging Face. À gauche figurent l'avatar et les informations de profil de l'utilisateur ; à droite se trouve la zone « User Access Tokens », qui explique que les jetons d'accès servent à l'authentification et permettent aux applications d'effectuer des opérations selon les permissions du jeton, en insistant sur le fait de ne pas partager le jeton avec d'autres. En haut à droite se trouve un bouton « + Create new token » pour créer un nouveau jeton. Cette image concerne la section « Créer un jeton » du document et présente visuellement le point d'entrée pour créer un jeton.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![Cette image montre la page de création d'un nouveau jeton d'accès (Token) sur la plateforme Hugging Face. Dans la page, la zone « Token type » met en évidence l'option « Write », le champ « Token name » est rempli avec « lerobot_zihao_token », et en bas se trouve un bouton « Create token » pour soumettre la création. À gauche de la page s'affichent les informations du compte actuellement connecté.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![L'image montre la fenêtre « Save your Access Token » du site Hugging Face. La fenêtre rappelle à l'utilisateur de conserver la valeur du jeton en lieu sûr, car elle ne pourra plus être consultée après fermeture. En dessous s'affiche la valeur du jeton « <your-huggingface-token> », avec un bouton « Copy » pour copier le jeton. On y voit aussi le nom du jeton « lerobot_zihao_token » et la permission « WRITE ». Cette image concerne l'étape « Créer un jeton » du document et sert à guider l'utilisateur pour enregistrer le jeton généré.](../../en/images/d24-03.png)
</column>
</grid>

## Enregistrer le jeton

Par exemple, le mien est :

```Shell
<your-huggingface-token>
```

## Associer le jeton

```Shell
hf auth login

hf auth whoami
```

![L'image montre l'utilisation d'un compte Hugging Face en ligne de commande sous macOS. La commande « hf auth login » est d'abord exécutée, indiquant qu'un jeton existe déjà et qu'on peut utiliser « hf auth whoami » pour obtenir des informations ou « hf auth logout » pour se déconnecter. Le jeton est ensuite saisi ; après confirmation, il est indiqué que le jeton a été enregistré dans le chemin spécifié et que le jeton actuellement actif est « lerobot zihao token ». Enfin, la commande « hf auth whoami » est exécutée et affiche l'utilisateur « TommyZihao ». Cette image correspond à la section « Créer un jeton » du document et présente visuellement l'opération de création du jeton et son résultat.](../../en/images/d24-04.png)

## Créer un dépôt de jeu de données

<grid>
<column width-ratio="0.480604">
![Il s'agit d'une capture d'écran de la plateforme Hugging Face. En haut à gauche figurent des entrées telles que Community, Docs et Pricing ; les informations du compte personnel TommyZihao se trouvent dans la zone centrale, et un cadre vert marque l'icône d'avatar de l'utilisateur en haut à droite. Dans le menu central, un cadre rouge met en évidence l'option « + New Dataset », qui correspond à l'étape « Créer un dépôt de jeu de données » du contexte — cliquer dessus lance la création d'un dépôt de jeu de données.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![L'image montre l'interface de création d'un nouveau dépôt de jeu de données sur Hugging Face. Le champ « Dataset name » est rempli avec « lerobot_zihao_dataset_a », la « License » est définie sur « mit », et l'option « Public » est cochée. En bas se trouve un bouton « Create dataset ». Cette image correspond à la section « Créer un dépôt de jeu de données » du document, présentant visuellement les informations clés à saisir lors de la création d'un dépôt de jeu de données et aidant l'utilisateur à comprendre l'interface et les champs requis.](../../en/images/d24-06.png)
</column>
</grid>

![Il s'agit de l'interface web de la plateforme Hugging Face, montrant le contenu d'un dépôt de jeu de données nommé lexobot_zihao_dataset_a créé par l'utilisateur TommyZhao, en thème sombre. À gauche se trouve l'option « Edit Dataset Card », et il est également proposé plusieurs façons de pousser les fichiers du jeu de données, avec des exemples de commandes pour CLI, Python, HTTPS et SSH, tout en indiquant que la License du jeu de données est mit.](../../en/images/d24-07.png)
