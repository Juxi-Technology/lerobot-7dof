[English](../en/09-train-model/train-wall-oss.md) | [简体中文](../zh-hans/09-train-model/train-wall-oss.md) | [繁體中文](../zh-hant/09-train-model/train-wall-oss.md) | [Deutsch](../de/09-train-model/train-wall-oss.md) | [Español](../es/09-train-model/train-wall-oss.md) | [Français](../fr/09-train-model/train-wall-oss.md) | [Italiano](../it/09-train-model/train-wall-oss.md) | [日本語](../ja/09-train-model/train-wall-oss.md) | [한국어](../ko/09-train-model/train-wall-oss.md) | [Português (BR)](../pt-br/09-train-model/train-wall-oss.md) | Português (PT)

# Linha de Comandos de Treino - WALL-OSS (Uma Luz Brilhante do Open Source Chinês)

## Documentação de Referência

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/walloss.mdx

## Instalar o Ambiente

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

## Linha de Comandos de Treino

```Shell
python lerobot/src/lerobot/scripts/lerobot_train.py \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --policy.type=wall_x \
    --output_dir=~/output_lerobot_train/shake/wallx_A \
    --job_name=shake_wallx_a \
    --policy.pretrained_name_or_path=x-square-robot/wall-oss-flow \
    --policy.prediction_mode=diffusion \
    --policy.attn_implementation=eager \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project \
    --steps=30000 \
    --policy.device=cuda \
    --batch_size=8
```

O arquivo do modelo tem cerca de 7.2G
