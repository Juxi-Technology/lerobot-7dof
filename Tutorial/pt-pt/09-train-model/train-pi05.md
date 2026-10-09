[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | Português (PT)

# Linha de Comandos de Treino - pi0.5

## Documentação de Referência

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## Instância de GPU na Nuvem Recomendada

![Esta imagem mostra os detalhes de uma instância de GPU na nuvem RTX A6000 oferecida pela Alibaba Cloud. O seu preço de pagamento conforme o uso é de 3.29 CNY/hora e estão disponíveis 3 placas. A instância está configurada com uma GPU RTX A6000 com um total de 51.0 GB de memória de GPU, uma CPU AMD EPYC 7742 de 30 núcleos, 60.9 GB de memória e 429.5 GB de disco. Há um botão azul "Start Using" na parte inferior. A imagem relaciona-se com a secção "Instância de GPU na Nuvem Recomendada", apresentando visualmente a configuração de GPU na nuvem recomendada, o preço e outra informação essencial.](../../en/images/d40-01.png)

## Instalar o Ambiente

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## Linha de Comandos

- Eliminar os ficheiros em output deixados pela execução de treino interrompida anteriormente

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- Treinar

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
![Esta imagem mostra a saída ao treinar um modelo de inteligência física (PI) a partir da linha de comandos. Mostra o modelo a ser carregado, parâmetros a serem remapeados e o otimizador e o scheduler a serem criados, por exemplo "Loading model from: lerobot/pi05_base". Aparecem também mensagens de aviso, como "Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy". Apresenta ainda números relacionados com o treino, como "num_total_frames: 180K". A imagem relaciona-se com a operação de treino a partir da linha de comandos, apresentando visualmente a informação essencial da execução do treino.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![Esta imagem mostra a saída durante o treino a partir da linha de comandos. Durante o treino, o processo huggingface/torch é bifurcado (fork); como o paralelismo já está em uso, o paralelismo é desativado para evitar um impasse (deadlock), com uma mensagem a aconselhar evitar fazê-lo "before the fork if possible". O treino só começa efetivamente após 20 minutos. A imagem está estreitamente relacionada com o contexto, apresentando as operações do processo e as mensagens de aviso que podem surgir durante o treino e ajudando a explicar o estado e os aspetos a que deve estar atento.](../../en/images/d40-02.png)
</column>
</grid>

Depois de a linha de comandos estar em execução durante 20 minutos, o treino só então começa efetivamente

O arquivo do modelo tem cerca de 5 GB e cerca de 7 GB depois de extraído
