[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | Português (PT)

# Linha de Comandos de Treino - ACT (Recomendado para Principiantes)

## Documentação de Referência

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## Porquê Começar pelo Algoritmo ACT

O ACT é o primeiro modelo mais recomendado para treinar quando se inicia no LeRobot. As suas vantagens são:

- O modelo é muito leve, com apenas 80 milhões de parâmetros treináveis
- O treino converge rapidamente e a inferência também é rápida
- Vê resultados após apenas uma hora de treino numa única GPU
- O arquivo do modelo ACT tem cerca de 200 MB, o que facilita o armazenamento e a transferência
- Recolher cerca de 30 episódios de dados costuma ser suficiente
- Pode ser implementado para inferência num anfitrião Ubuntu, num Mac, num PC Windows, até num Raspberry Pi
- A inferência num robô real funciona bastante bem e é mais do que suficiente para tarefas simples como apanhar, apertar a mão e colocar uma caneta
- O algoritmo ACT já vem integrado no ambiente base do LeRobot, pelo que não são necessárias bibliotecas adicionais

## Linha de Comandos

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

## Notas sobre a Linha de Comandos

Um `\` de continuação de linha só pode ter um espaço antes de si e nenhum espaço depois

Os parâmetros apresentados a vermelho têm de ser verificados ou alterados antes de cada execução

| Parâmetro da linha de comandos | Descrição |
|-|-|
| --dataset.repo_id | Repo_ID do conjunto de dados do HuggingFace |
| --dataset.root | Caminho local para o conjunto de dados |
| --dataset.revision | Versão do conjunto de dados, indicada quando carregou o conjunto de dados para o HuggingFace |
| --dataset.streaming | O conjunto de dados é local, pelo que isto tem de ser `false`, uma vez que os dados já estão em disco e não é necessária leitura em streaming |
| --dataset.split | Assume `train` por defeito, o que significa que todo o conjunto de dados é utilizado como conjunto de treino |
| --policy.type | O algoritmo a treinar, como act, smolvla, diffusion, pi0, wallx |
| --output_dir | Diretório onde é guardada a estrutura de saída |
| --job_name | Nome desta tarefa de treino |
| --policy.device | Dispositivo de computação |
| --wandb.enable | Ativar a visualização wandb |
| --wandb.project | Nome do projeto wandb |
| --policy.push_to_hub | Enviar o modelo treinado para o HuggingFace |
| --steps | Número de passos de treino |
| --batch_size | Quantidade de dados introduzidos por passo; reduza-a se ficar sem memória de GPU |
|  |  |

## Processo de Treino

<grid>
<column width-ratio="0.357753">
![Esta imagem mostra um exemplo de utilização do comando de treino `lerobot-train` a partir da linha de comandos. O comando define vários parâmetros, como `--dataset.repo_id` e `--dataset.root`, para especificar os detalhes do conjunto de dados, define `--policy.type` como `act` e `--output_dir` como o diretório de saída `outputs/lerobot_train/output_a`, a par de outros parâmetros como `--job_name` e `--policy.device`. Lista também os valores predefinidos de parâmetros como `--dataset.split` e `--policy.push_to_hub`. A imagem está estreitamente relacionada com o contexto, mostrando visualmente como são definidos os parâmetros do comando de treino.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![Esta imagem mostra a saída da linha de comandos durante o treino. Apresenta detalhes do treino do modelo, como as definições de scheduler, steps e use_policy_training_preset, a par de parâmetros relacionados com o conjunto de dados. Mostra também informação como o número de parâmetros do modelo e a loss, por exemplo num_total_params de 55917096 (52M) e uma loss de 0.626. Por baixo está informação de descarregamento de ficheiros, como descarregar "https://download.pytorch.org/models/resnet18-f37072fd.pth" para o diretório /home/featurize/.cache/torch/hub/checkpoints. A imagem relaciona-se com a linha de comandos de treino descrita no contexto, mostrando visualmente o que a linha de comandos apresenta durante o treino.](../../en/images/d36-02.png)
</column>
</grid>

![Esta imagem mostra a informação de registo produzida durante o treino. O registo regista vários passos de treino por ordem cronológica, incluindo a hora, o número de iteração do treino, a loss e a taxa de aprendizagem — por exemplo, às 15:11:53 de 14 de janeiro de 2024 o número de iteração era 131k e a loss era 0.368. Aqui `INFO` é o tipo de registo, `train` a fase de treino, `step` o número de iteração, `loss` o valor da loss e `lr` a taxa de aprendizagem. A imagem relaciona-se com a secção de notas sobre a linha de comandos do documento, apresentando visualmente os dados essenciais da execução do treino.](../../en/images/d36-03.png)

O arquivo do modelo tem cerca de 300 MB
