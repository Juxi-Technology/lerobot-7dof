[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | [Deutsch](../de/09-train-model/train-pi0fast.md) | [Español](../es/09-train-model/train-pi0fast.md) | [Français](../fr/09-train-model/train-pi0fast.md) | [Italiano](../it/09-train-model/train-pi0fast.md) | [日本語](../ja/09-train-model/train-pi0fast.md) | [한국어](../ko/09-train-model/train-pi0fast.md) | Português (BR) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# Linha de comando de treinamento - pi0fast

## Documentação de referência

https://huggingface.co/docs/lerobot/pi0fast

## Problema

https://github.com/huggingface/lerobot/pull/2203

## Instância de GPU em nuvem recomendada

![Esta imagem mostra os detalhes da instância de GPU em nuvem RTX A6000 recomendada. Ela mostra que 3 placas estão disponíveis e que o preço de pagamento conforme o uso é 3.29 CNY/hora. A configuração é uma GPU RTX A6000 com um total de 51.0 GB de memória de GPU, uma CPU AMD EPYC 7742 de 30 núcleos, 60.9 GB de memória e 429.5 GB de disco, com um botão "Start Using" na parte inferior. A imagem fica na seção "Instância de GPU em nuvem recomendada", dando ao usuário a recomendação de GPU em nuvem e sua configuração principal e preço para o treinamento.](../../en/images/d41-01.png)

## Instale o ambiente

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## Linha de comando

- Exclua os arquivos sob output deixados pela execução de treinamento interrompida anterior

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- Treinar

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




## Conteúdo anterior

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









Após a execução, o treinamento só começa de verdade depois de cerca de 10 minutos

O arquivo do modelo tem cerca de 5 GB, e cerca de 7 GB depois de extraído
