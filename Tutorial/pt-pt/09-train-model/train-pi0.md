[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | [Deutsch](../de/09-train-model/train-pi0.md) | [Español](../es/09-train-model/train-pi0.md) | [Français](../fr/09-train-model/train-pi0.md) | [Italiano](../it/09-train-model/train-pi0.md) | [日本語](../ja/09-train-model/train-pi0.md) | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | Português (PT)

# Linha de Comandos de Treino - pi0 (Melhores Resultados)

## Documentação de Referência

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## Instância de GPU na Nuvem Recomendada

![Esta imagem mostra os detalhes de uma instância de GPU na nuvem RTX A6000. O seu preço de pagamento conforme o uso é de 3.29 CNY/hora; a GPU é uma RTX A6000 com um total de 51.0 GB de memória de GPU; a CPU é uma AMD EPYC 7742 de 30 núcleos; a memória é de 60.9 GB; e o disco é de 429.5 GB. No canto superior direito mostra que estão disponíveis 3 placas. Há um botão azul "Start Using" na parte inferior. A imagem relaciona-se com a secção "Instância de GPU na Nuvem Recomendada", apresentando visualmente a configuração de GPU na nuvem recomendada, o preço e outra informação essencial.](../../en/images/d39-01.png)

## Instalar o Ambiente

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## Linha de Comandos

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

Depois de a linha de comandos estar em execução durante 20 minutos, o treino só então começa efetivamente

O arquivo do modelo tem cerca de 5 GB e cerca de 7 GB depois de extraído
