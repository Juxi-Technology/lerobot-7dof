[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | Português (BR) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# Linha de comando de treinamento - ACT (Recomendado para iniciantes)

## Documentação de referência

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## Por que começar com o algoritmo ACT

O ACT é o primeiro modelo mais recomendado para treinar quando você começa no LeRobot. Suas vantagens são:

- O modelo é muito leve, com apenas 80 milhões de parâmetros treináveis
- O treinamento converge rapidamente, e a inferência também é rápida
- Você vê resultados depois de apenas uma hora de treinamento em uma única GPU
- O arquivo do modelo ACT tem cerca de 200 MB, o que facilita o armazenamento e a transferência
- Coletar cerca de 30 episódios de dados costuma ser suficiente
- Ele pode ser implantado para inferência em um host Ubuntu, um Mac, um PC Windows, até um Raspberry Pi
- A inferência em um robô real funciona muito bem, e é mais do que suficiente para tarefas simples como pegar objetos, apertar as mãos e colocar canetas
- O algoritmo ACT já vem embutido no ambiente base do LeRobot, então nenhuma biblioteca extra é necessária

## Linha de comando

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## Observações sobre a linha de comando

Um `\` de continuação de linha pode ter apenas um espaço antes e nenhum espaço depois

Os parâmetros mostrados em vermelho precisam ser verificados ou alterados antes de cada execução

| Parâmetro da linha de comando | Descrição |
|-|-|
| --dataset.repo_id | Repo_ID do conjunto de dados do HuggingFace |
| --dataset.root | Caminho local para o conjunto de dados |
| --dataset.revision | Versão do conjunto de dados, especificada quando você enviou o conjunto de dados para o HuggingFace |
| --dataset.streaming | O conjunto de dados é local, então isto precisa ser `false`, já que os dados já estão em disco e nenhuma leitura em streaming é necessária |
| --dataset.split | Tem como padrão `train`, o que significa que todo o conjunto de dados é usado como conjunto de treinamento |
| --policy.type | O algoritmo a treinar, como act, smolvla, diffusion, pi0, wallx |
| --output_dir | Diretório onde a estrutura de saída é salva |
| --job_name | Nome deste trabalho de treinamento |
| --policy.device | Dispositivo de computação |
| --wandb.enable | Ativa a visualização no wandb |
| --wandb.project | Nome do projeto no wandb |
| --policy.push_to_hub | Envia o modelo treinado para o HuggingFace |
| --steps | Número de passos de treinamento |
| --batch_size | Quantidade de dados alimentados por passo; reduza se ficar sem memória de GPU |
|  |  |

## Processo de treinamento

<grid>
<column width-ratio="0.357753">
![Esta imagem mostra um exemplo de uso do comando de treinamento `lerobot-train` a partir da linha de comando. O comando define vários parâmetros, como `--dataset.repo_id` e `--dataset.root`, para especificar detalhes do conjunto de dados, define `--policy.type` como `act` e `--output_dir` como o diretório de saída `outputs/lerobot_train/output_a`, além de outros parâmetros como `--job_name` e `--policy.device`. Ele também lista os valores padrão de parâmetros como `--dataset.split` e `--policy.push_to_hub`. A imagem está intimamente relacionada ao contexto, mostrando visualmente como os parâmetros do comando de treinamento são definidos.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![Esta imagem mostra a saída da linha de comando durante o treinamento. Ela exibe detalhes do treinamento do modelo, como as configurações de scheduler, steps e use_policy_training_preset, junto com parâmetros relacionados ao conjunto de dados. Também mostra informações como o número de parâmetros do modelo e a loss, por exemplo num_total_params de 55917096 (52M) e uma loss de 0.626. Abaixo há informações de download de arquivos, como o download de "https://download.pytorch.org/models/resnet18-f37072fd.pth" para o diretório /home/featurize/.cache/torch/hub/checkpoints. A imagem está relacionada à linha de comando de treinamento descrita no contexto, mostrando visualmente o que a linha de comando gera durante o treinamento.](../../en/images/d36-02.png)
</column>
</grid>

![Esta imagem mostra as informações de log produzidas durante o treinamento. O log registra vários passos de treinamento em ordem cronológica, incluindo o horário, a contagem de iteração do treinamento, a loss e a taxa de aprendizado — por exemplo, às 15:11:53 de 14 de janeiro de 2024 a contagem de iteração era 131k e a loss era 0.368. Aqui `INFO` é o tipo de log, `train` a fase de treinamento, `step` a contagem de iteração, `loss` o valor da loss e `lr` a taxa de aprendizado. A imagem está relacionada à seção de observações sobre a linha de comando do documento, apresentando visualmente os dados-chave da execução do treinamento.](../../en/images/d36-03.png)

O arquivo do modelo tem cerca de 300 MB
