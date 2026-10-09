[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | Français | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# Ligne de commande d'entraînement - pi0.5

## Documentation de référence

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## Instance GPU cloud recommandée

![Cette image montre les détails d'une instance GPU cloud RTX A6000 proposée par Alibaba Cloud. Son prix à la demande est de 3.29 CNY/heure, et 3 cartes sont disponibles. L'instance est configurée avec un GPU RTX A6000 totalisant 51.0 GB de mémoire GPU, un CPU AMD EPYC 7742 à 30 cœurs, 60.9 GB de mémoire et 429.5 GB de disque. Un bouton bleu « Start Using » figure en bas. L'image se rapporte à la section « Instance GPU cloud recommandée », présentant visuellement la configuration GPU cloud recommandée, son prix et d'autres informations clés.](../../en/images/d40-01.png)

## Installer l'environnement

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## Ligne de commande

- Supprimer les fichiers présents dans le répertoire output, laissés par la session d'entraînement interrompue précédente

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- Entraîner

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi05 \
    --output_dir=~/output_lerobot_train/shake/pi05_A \
    --job_name=shake_pi05_A \
    --policy.pretrained_path=lerobot/pi05_base \
    --policy.compile_model=true \
    --policy.gradient_checkpointing=true \
    --policy.dtype=bfloat16 \
    --policy.freeze_vision_encoder=false \
    --policy.train_expert_only=false \
    --steps=50000 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project \
    --batch_size=8
```

<grid>
<column width-ratio="0.468128">
![Cette image montre la sortie produite lors de l'entraînement d'un modèle d'intelligence physique (PI) depuis la ligne de commande. Elle montre le modèle en cours de chargement, le remappage des paramètres, et la création de l'optimiseur et du planificateur, par exemple « Loading model from: lerobot/pi05_base ». Des messages d'avertissement apparaissent aussi, tels que « Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy ». Elle présente également des chiffres liés à l'entraînement comme « num_total_frames: 180K ». L'image se rapporte à l'opération d'entraînement en ligne de commande, présentant visuellement les informations clés de la session d'entraînement.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![Cette image montre la sortie pendant l'entraînement en ligne de commande. Pendant l'entraînement, le processus huggingface/torch est dupliqué (fork) ; comme le parallélisme est déjà utilisé, il est désactivé pour éviter un interblocage, avec un message conseillant d'éviter de faire cela « before the fork if possible ». L'entraînement ne commence vraiment qu'au bout de 20 minutes. L'image est étroitement liée au contexte, présentant les opérations du processus et les messages de conseil qui peuvent apparaître pendant l'entraînement, et aidant à expliquer l'état et les points de vigilance.](../../en/images/d40-02.png)
</column>
</grid>

Une fois la ligne de commande lancée depuis 20 minutes, l'entraînement ne commence vraiment qu'à ce moment-là

L'archive du modèle fait environ 5 GB, et environ 7 GB une fois décompressée
