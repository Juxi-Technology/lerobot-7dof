[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | Português (PT)

<title>Carregar conjunto de dados para o HuggingFace (opcional)</title>

# Método 1: Carregar localmente (não recomendado; velocidade de carregamento lenta)

- Carregamento automático

Defina `push_to_hub=true` ao recolher o conjunto de dados, e este é carregado automaticamente quando a recolha termina

![Esta imagem mostra uma interface da linha de comandos, parte do registo de execução do processo de carregamento do conjunto de dados. No topo mostra informação de ambiente para ferramentas como SVN e treet W2; ao centro há uma mensagem de processamento como "Starting the second pass: moving the mov atom to the beginning of the file", e em baixo há erros da execução como "error messaging the mach port for IMCRunLoopWakeUpReliable", enquanto à direita lista o progresso de processamento e números de transferência de dados, como a quantidade de dados e a velocidade de diferentes entradas. De um modo geral, apresenta um registo do estado de execução durante o processamento do carregamento do conjunto de dados.](../../en/images/d28-01.png)

- Carregamento manual

Defina `push_to_hub=false` ao recolher o conjunto de dados, e carregue manualmente quando a recolha termina

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



Quer seja automático ou manual, a velocidade de carregamento é muito lenta (cerca de 100 KB por segundo)

porque os servidores da HuggingFace estão no estrangeiro

# Método 2: Carregar a partir de uma plataforma de GPU na nuvem (recomendado)

## Iniciar sessão na plataforma de GPU na nuvem Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## Iniciar uma instância de GPU na nuvem

## Carregar o arquivo do conjunto de dados para `Datasets`

## Copiar o comando de transferência da instância

![A imagem mostra a página de conjuntos de dados da plataforma Featurize. No topo mostra o título "Datasets", e em baixo está um conjunto de dados com o nome "soarm_amazing_hand_pick.zip", de 213.3 MB, carregado há 16 horas. À direita há um botão "Cloud Unzip", juntamente com botões como "Like", "Comment" e "Copy Instance Download Command". Esta imagem está relacionada com o passo "Carregar o arquivo do conjunto de dados para `Datasets`", mostrando a página de conjuntos de dados após o carregamento.](../../en/images/d28-02.png)

## Executar na linha de comandos da instância de GPU na nuvem

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## Carregar o conjunto de dados para o HuggingFace

Crie um ficheiro `upload_dataset.py` com o seguinte conteúdo

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

Executar o ficheiro

```Shell
python upload_dataset.py
```

![Esta imagem mostra o processo de executar o carregamento do conjunto de dados na linha de comandos de uma instância de GPU na nuvem, em que um utilizador chamado lerobot2 executou o comando python upload.py. Mostra o progresso de processamento dos ficheiros, com 6 ficheiros a processar e todos a 100% de progresso, e assinala o tamanho de transferência de cada ficheiro, com o progresso total de transferência de dados a 100%. Na parte inferior indica que nenhum ficheiro foi modificado desde o último commit, pelo que o commit é ignorado para evitar criar um commit vazio. Este conteúdo corresponde ao passo de executar o ficheiro upload_dataset.py.](../../en/images/d28-03.png)

- Outro método de carregamento (não recomendado)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![Esta imagem mostra o processo de carregar um conjunto de dados utilizando o comando `hf upload` do HuggingFace, com o comando `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`. A imagem mostra que o carregamento entrou na sua fase final, com todos os ficheiros a 100% de progresso, incluindo vários ficheiros de vídeo e ficheiros parquet cujos tamanhos de carregamento correspondem exatamente aos tamanhos dos ficheiros locais correspondentes, juntamente com o tamanho total dos ficheiros carregados e a velocidade de transferência e, na parte inferior, uma ligação para a página do conjunto de dados no HuggingFace referente a este commit de carregamento, indicando que a tarefa de carregamento está concluída.](../../en/images/d28-04.png)

# Ver o conjunto de dados no HuggingFace

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![Esta imagem é uma captura de ecrã da página de detalhes do conjunto de dados soarm_amazing_hand_pick da equipa Juxi-Technology na plataforma Hugging Face, correspondendo ao conteúdo "Ver o conjunto de dados no HuggingFace". No topo mostra opções de navegação do conjunto de dados e informação básica como o autor e as etiquetas; ao centro, a área Dataset Viewer mostra parte dos dados de treino da split 1 do conjunto de dados, incluindo campos como action, observation_state e timestamp e os respetivos valores, e assinala também o tamanho de um único registo, o número total de registos e o tamanho total. Na parte inferior menciona também modelos relacionados treinados com estes dados.](../../en/images/d28-05.png)

![A imagem mostra a página do conjunto de dados soarm_amazing_hand_pick na plataforma Hugging Face. No topo há uma caixa de pesquisa e uma barra de navegação para pesquisar modelos, conjuntos de dados, etc. A secção de informação do conjunto de dados mostra a organização proprietária Juxi - Technology e etiquetas como robotics e imitation-learning. Sob o separador "Files and versions" lista pastas como data, meta e videos e o ficheiro README.md, mostrando o autor do carregamento, o método de carregamento e a hora, como "Upload README.md with huggingface_hub". Esta imagem está relacionada com ver um conjunto de dados no HuggingFace, apresentando visualmente os ficheiros e as versões do conjunto de dados.](../../en/images/d28-06.png)
