[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | Português (BR) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# Linha de comando de treinamento - pi0.5

## Documentação de referência

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## Instância de GPU em nuvem recomendada

![Esta imagem mostra os detalhes de uma instância de GPU em nuvem RTX A6000 oferecida pela Alibaba Cloud. Seu preço de pagamento conforme o uso é 3.29 CNY/hora, e 3 placas estão disponíveis. A instância está configurada com uma GPU RTX A6000 com um total de 51.0 GB de memória de GPU, uma CPU AMD EPYC 7742 de 30 núcleos, 60.9 GB de memória e 429.5 GB de disco. Há um botão azul "Start Using" na parte inferior. A imagem está relacionada à seção "Instância de GPU em nuvem recomendada", apresentando visualmente a configuração recomendada de GPU em nuvem, o preço e outras informações-chave.](../../en/images/d40-01.png)

## Instale o ambiente

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## Linha de comando

- Exclua os arquivos sob output deixados pela execução de treinamento interrompida anterior

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
![Esta imagem mostra a saída ao treinar um modelo de inteligência física (PI) a partir da linha de comando. Ela mostra o modelo sendo carregado, os parâmetros sendo remapeados e o otimizador e o scheduler sendo criados, por exemplo "Loading model from: lerobot/pi05_base". Também aparecem mensagens de aviso, como "Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy". Ela também apresenta números relacionados ao treinamento, como "num_total_frames: 180K". A imagem está relacionada à operação da linha de comando de treinamento, apresentando visualmente as informações-chave da execução do treinamento.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![Esta imagem mostra a saída durante o treinamento pela linha de comando. Durante o treinamento, o processo huggingface/torch é bifurcado (fork); como o paralelismo já está em uso, o paralelismo é desativado para evitar um deadlock, com uma mensagem aconselhando a evitar fazer isso "before the fork if possible". O treinamento só começa de verdade depois de 20 minutos. A imagem está intimamente relacionada ao contexto, apresentando as operações do processo e as mensagens de aviso que podem aparecer durante o treinamento, ajudando a explicar o estado e os pontos de atenção.](../../en/images/d40-02.png)
</column>
</grid>

Depois que a linha de comando estiver rodando por 20 minutos, o treinamento só então começa de verdade

O arquivo do modelo tem cerca de 5 GB, e cerca de 7 GB depois de extraído
