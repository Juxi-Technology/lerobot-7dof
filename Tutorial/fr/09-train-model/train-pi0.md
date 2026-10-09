[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | [Deutsch](../de/09-train-model/train-pi0.md) | [Español](../es/09-train-model/train-pi0.md) | Français | [Italiano](../it/09-train-model/train-pi0.md) | [日本語](../ja/09-train-model/train-pi0.md) | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# Ligne de commande d'entraînement - pi0 (meilleurs résultats)

## Documentation de référence

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## Instance GPU cloud recommandée

![Cette image montre les détails d'une instance GPU cloud RTX A6000. Son prix à la demande est de 3.29 CNY/heure ; le GPU est un RTX A6000 avec un total de 51.0 GB de mémoire GPU ; le CPU est un AMD EPYC 7742 à 30 cœurs ; la mémoire est de 60.9 GB ; et le disque de 429.5 GB. Le coin supérieur droit indique que 3 cartes sont disponibles. Un bouton bleu « Start Using » figure en bas. L'image se rapporte à la section « Instance GPU cloud recommandée », présentant visuellement la configuration GPU cloud recommandée, son prix et d'autres informations clés.](../../en/images/d39-01.png)

## Installer l'environnement

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## Ligne de commande

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_A

lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0 \
  --output_dir=~/output_lerobot_train/shake/pi0_A \
  --job_name=shake_pi0_A \
  --policy.pretrained_path=lerobot/pi0_base \
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

Une fois la ligne de commande lancée depuis 20 minutes, l'entraînement ne commence vraiment qu'à ce moment-là

L'archive du modèle fait environ 5 GB, et environ 7 GB une fois décompressée
