[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | Français | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# Téléverser un modèle vers HuggingFace (facultatif)

## Créer un dépôt de modèle

<grid>
<column width-ratio="0.354197">
![Cette image montre l'interface utilisateur de HuggingFace. On y voit une icône d'avatar ; un clic dessus ouvre un menu déroulant dans lequel l'option « New Model » est mise en évidence par un cadre rouge. L'image se rapporte à la section « Téléverser un modèle vers HuggingFace (facultatif) » et correspond à l'étape « Créer un dépôt de modèle », présentant visuellement le point d'entrée pour créer un nouveau modèle sur HuggingFace et aidant l'utilisateur à comprendre comment créer des ressources liées aux modèles sur la plateforme.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![Cette image montre l'interface de création d'un nouveau dépôt de modèle sur le site HuggingFace. La liste déroulante « Owner » est réglée sur « TommyZihao », le champ « Model name » contient « lerobot_zihao_model_a » et le champ « License » contient « mit ». En dessous figurent une option « Base template » et les choix de type de dépôt « Public » et « Private ». L'image se rapporte à la section « Créer un dépôt de modèle » et constitue un exemple de remplissage des informations lors de la création d'un dépôt de modèle.](../../en/images/d46-02.png)
</column>
</grid>

## Consulter le dépôt du modèle

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

Il est vide pour l'instant

![Cette image montre la page du modèle « TommyZihao/lerobot_zihao_model_a » sur la plateforme HuggingFace. À gauche se trouve un onglet « Model card » pour modifier la fiche du modèle. À droite, la section « Getting started with your model » explique comment commencer à utiliser le modèle, notamment en ajoutant les informations complètes du modèle et en poussant les fichiers du modèle. En dessous, la zone « Edit Model Card » permet d'ajouter la licence, la langue, le modèle de base et d'autres informations du modèle. En bas, la zone « Push your model files » propose plusieurs façons de téléverser les fichiers du modèle, notamment CLI, Python, Git, HTTPS et SSH. L'image se rapporte au téléversement d'un modèle vers HuggingFace, montrant les contrôles de la page.](../../en/images/d46-03.png)

![Cette image montre la page du dépôt de modèle TommyZihao/lerobot_zihao_model_a sur la plateforme HuggingFace. La page indique une taille de fichier de 1.54 KB, un contributeur et un historique de 1 commit, effectué il y a 9 minutes. Elle liste également les fichiers .gitattributes et README.md, de 1.52 KB et 24 Bytes respectivement, eux aussi issus du commit initial, il y a également 9 minutes. L'image se rapporte au téléversement d'un modèle vers HuggingFace, montrant l'aspect de la page après le téléversement du modèle.](../../en/images/d46-04.png)

## Téléverser le modèle

Créez un fichier `upload_model.py` avec le contenu suivant

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

Exécutez-le

```Shell
python upload_model.py
```

![Cette image montre la sortie de l'exécution de la commande `python upload_model.py` depuis la ligne de commande. Elle affiche la progression du traitement des fichiers à 34 % et la progression du téléversement des nouvelles données également à 34 %, et liste la progression de téléversement des deux fichiers `d_model/model.safetensors` et `tokenizer_processor.safetensors` à 92 % chacun. L'image se rapporte au téléversement d'un modèle vers HuggingFace, montrant visuellement la progression lors du téléversement des fichiers du modèle.](../../en/images/d46-05.png)

## Consulter le dépôt du modèle

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![Cette image montre la page du dépôt HuggingFace du modèle lerobot_zihao_model_a de TommyZihao. La page indique la licence du modèle comme mit, un contributeur et un historique de 2 commits. Au milieu, elle liste plusieurs fichiers, tels que README.md, config.json et model.safetensors, chacun accompagné du texte « Upload folder using huggingface_hub » sur sa droite, indiquant que ces fichiers ont été téléversés via huggingface_hub. L'image se rapporte au téléversement d'un modèle vers HuggingFace, montrant visuellement comment les fichiers du modèle sont stockés sur HuggingFace.](../../en/images/d46-06.png)

Les fichiers du modèle sont maintenant en place
