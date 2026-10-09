[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | Português (BR) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>Enviar o Conjunto de Dados para o HuggingFace (Opcional)</title>

# Método 1: Enviar Localmente (Não Recomendado; Velocidade de Upload Lenta)

- Upload automático

Defina `push_to_hub=true` ao coletar o conjunto de dados, e ele será enviado automaticamente assim que a coleta terminar

![Esta imagem mostra uma interface de linha de comando, parte do log de execução do processo de upload do conjunto de dados. No topo, ela mostra informações de ambiente para ferramentas como SVN e treet W2; no meio há uma mensagem de processamento como "Starting the second pass: moving the mov atom to the beginning of the file", e abaixo há erros da execução como "error messaging the mach port for IMCRunLoopWakeUpReliable", enquanto à direita lista o progresso de processamento e os números de transferência de dados, como a quantidade de dados e a velocidade para diferentes entradas. No geral, apresenta um registro do estado de execução durante o processamento do upload do conjunto de dados.](../../en/images/d28-01.png)

- Upload manual

Defina `push_to_hub=false` ao coletar o conjunto de dados, e faça o upload manualmente assim que a coleta terminar

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



Seja automático ou manual, a velocidade de upload é muito lenta (cerca de 100 KB por segundo)

porque os servidores do HuggingFace ficam no exterior

# Método 2: Enviar a Partir de uma Plataforma de GPU na Nuvem (Recomendado)

## Faça Login na Plataforma de GPU na Nuvem Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## Inicie uma Instância de GPU na Nuvem

## Envie o Arquivo do Conjunto de Dados para `Datasets`

## Copie o Comando de Download da Instância

![A imagem mostra a página de datasets da plataforma Featurize. No topo, ela exibe o título "Datasets", e abaixo há um conjunto de dados chamado "soarm_amazing_hand_pick.zip", de 213.3 MB, enviado há 16 horas. À direita há um botão "Cloud Unzip", juntamente com botões como "Like", "Comment" e "Copy Instance Download Command". Esta imagem está relacionada à etapa "Envie o arquivo do conjunto de dados para `Datasets`", mostrando a página de datasets após o upload.](../../en/images/d28-02.png)

## Execute na Linha de Comando da Instância de GPU na Nuvem

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## Envie o Conjunto de Dados para o HuggingFace

Crie um arquivo `upload_dataset.py` com o seguinte conteúdo

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

Execute o arquivo

```Shell
python upload_dataset.py
```

![Esta imagem mostra o processo de execução do upload do conjunto de dados na linha de comando de uma instância de GPU na nuvem, em que um usuário chamado lerobot2 executou o comando python upload.py. Ela mostra o progresso de processamento dos arquivos, com 6 arquivos a processar e todos com 100% de progresso, e marca o tamanho de transferência de cada arquivo, com o progresso total de transferência de dados em 100%. Na parte inferior, observa que nenhum arquivo foi modificado desde o último commit, então o commit é ignorado para evitar criar um commit vazio. Este conteúdo corresponde à etapa de executar o arquivo upload_dataset.py.](../../en/images/d28-03.png)

- Outro método de upload (não recomendado)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![Esta imagem mostra o processo de envio de um conjunto de dados usando o comando `hf upload` do HuggingFace, com o comando `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`. A imagem mostra que o upload entrou em sua fase final, com todos os arquivos em 100% de progresso, incluindo vários arquivos de vídeo e arquivos parquet cujos tamanhos de upload correspondem exatamente aos tamanhos dos arquivos locais correspondentes, juntamente com o tamanho total dos arquivos enviados e a velocidade de transferência, e, na parte inferior, um link para a página do conjunto de dados no HuggingFace referente a este commit de upload, indicando que a tarefa de upload foi concluída.](../../en/images/d28-04.png)

# Visualizar o Conjunto de Dados no HuggingFace

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![Esta imagem é uma captura de tela da página de detalhes do conjunto de dados soarm_amazing_hand_pick da equipe Juxi-Technology na plataforma Hugging Face, correspondendo ao conteúdo "Visualizar o Conjunto de Dados no HuggingFace". No topo, ela mostra opções de navegação do conjunto de dados e informações básicas como o autor e as tags; no meio, a área Dataset Viewer mostra parte dos dados de treino do split 1 do conjunto de dados, incluindo campos como action, observation_state e timestamp e seus valores, e também marca o tamanho de um único registro, o número total de registros e o tamanho total. Na parte inferior, ela também menciona modelos relacionados treinados com esses dados.](../../en/images/d28-05.png)

![A imagem mostra a página do conjunto de dados soarm_amazing_hand_pick na plataforma Hugging Face. No topo há uma caixa de pesquisa e uma barra de navegação para pesquisar modelos, conjuntos de dados e assim por diante. A seção de informações do conjunto de dados mostra a organização proprietária Juxi - Technology e tags como robotics e imitation-learning. Na aba "Files and versions", ela lista pastas como data, meta e videos e o arquivo README.md, mostrando o autor do upload, o método de upload e o horário, como "Upload README.md with huggingface_hub". Esta imagem está relacionada à visualização de um conjunto de dados do HuggingFace, apresentando visualmente os arquivos e as versões do conjunto de dados.](../../en/images/d28-06.png)
