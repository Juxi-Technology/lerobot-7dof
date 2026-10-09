[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | [Deutsch](../de/09-train-model/train-pi0fast.md) | [Español](../es/09-train-model/train-pi0fast.md) | Français | [Italiano](../it/09-train-model/train-pi0fast.md) | [日本語](../ja/09-train-model/train-pi0fast.md) | [한국어](../ko/09-train-model/train-pi0fast.md) | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# Ligne de commande d'entraînement - pi0fast

## Documentation de référence

https://huggingface.co/docs/lerobot/pi0fast

## Problème

https://github.com/huggingface/lerobot/pull/2203

## Instance GPU cloud recommandée

![Cette image montre les détails de l'instance GPU cloud RTX A6000 recommandée. Elle indique que 3 cartes sont disponibles et que le prix à la demande est de 3.29 CNY/heure. La configuration est un GPU RTX A6000 totalisant 51.0 GB de mémoire GPU, un CPU AMD EPYC 7742 à 30 cœurs, 60.9 GB de mémoire et 429.5 GB de disque, avec un bouton « Start Using » en bas. L'image se situe dans la section « Instance GPU cloud recommandée », fournissant à l'utilisateur la recommandation de GPU cloud et sa configuration clé et son prix pour l'entraînement.](../../en/images/d41-01.png)

## Installer l'environnement

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## Ligne de commande

- Supprimer les fichiers présents dans le répertoire output, laissés par la session d'entraînement interrompue précédente

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- Entraîner

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi0_fast \
    --output_dir=output_lerobot_train/shake/pi0_fast_A \
    --job_name=shake_pi0_fast_A \
    --policy.pretrained_path=lerobot/pi0_fast_base \
    --policy.dtype=bfloat16 \
    --policy.gradient_checkpointing=true \
    --policy.chunk_size=10 \
    --policy.n_action_steps=10 \
    --policy.max_action_tokens=256 \
    --steps=50000 \
    --batch_size=8 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project
```





## Contenu précédent

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0_fast \
  --output_dir=output_lerobot_train/shake/pi0_fast_A \
  --job_name=shake_pi0_fast_A \
  --policy.pretrained_path=lerobot/pi0_fast_base \
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









Après l'exécution, l'entraînement ne commence vraiment qu'au bout d'environ 10 minutes

L'archive du modèle fait environ 5 GB, et environ 7 GB une fois décompressée
