[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | Português (PT)

# Carregar um Modelo para o HuggingFace (Opcional)

## Criar um Repositório de Modelo

<grid>
<column width-ratio="0.354197">
![Esta imagem mostra a interface de utilizador do HuggingFace. Mostra um ícone de avatar; clicá-lo abre um menu pendente no qual a opção "New Model" está realçada com uma caixa vermelha. A imagem relaciona-se com a secção "Carregar um Modelo para o HuggingFace (Opcional)" e corresponde ao passo "Criar um Repositório de Modelo", apresentando visualmente o ponto de entrada para criar um novo modelo no HuggingFace e ajudando o utilizador a compreender como criar recursos relacionados com modelos na plataforma.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![Esta imagem mostra a interface para criar um novo repositório de modelo no sítio do HuggingFace. A lista pendente "Owner" está definida como "TommyZihao", o campo "Model name" contém "lerobot_zihao_model_a" e o campo "License" contém "mit". Por baixo estão uma opção "Base template" e as escolhas de tipo de repositório "Public" e "Private". A imagem relaciona-se com a secção "Criar um Repositório de Modelo" e é um exemplo do preenchimento dos detalhes ao criar um repositório de modelo.](../../en/images/d46-02.png)
</column>
</grid>

## Ver o Repositório de Modelo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

Está vazio por agora

![Esta imagem mostra a página do modelo "TommyZihao/lerobot_zihao_model_a" na plataforma HuggingFace. À esquerda está um separador "Model card" para editar o cartão do modelo. À direita, a secção "Getting started with your model" explica como começar a utilizar o modelo, incluindo adicionar informação completa do modelo e enviar ficheiros do modelo. Por baixo, a área "Edit Model Card" permite adicionar a Licença, o idioma, o modelo base e outra informação do modelo. Na parte inferior, a área "Push your model files" oferece várias formas de carregar ficheiros do modelo, incluindo CLI, Python, Git, HTTPS e SSH. A imagem relaciona-se com o carregamento de um modelo para o HuggingFace, mostrando os controlos da página.](../../en/images/d46-03.png)

![Esta imagem mostra a página do repositório de modelo TommyZihao/lerobot_zihao_model_a na plataforma HuggingFace. A página mostra o tamanho do ficheiro do modelo como 1.54 KB, um contribuidor e um histórico de 1 commit, feito há 9 minutos. Lista também os ficheiros .gitattributes e README.md, com 1.52 KB e 24 Bytes, respetivamente, igualmente do commit inicial, também há 9 minutos. A imagem relaciona-se com o carregamento de um modelo para o HuggingFace, mostrando o aspeto da página depois de o modelo ser carregado.](../../en/images/d46-04.png)

## Carregar o Modelo

Crie um ficheiro `upload_model.py` com o seguinte conteúdo

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

Execute-o

```Shell
python upload_model.py
```

![Esta imagem mostra a saída da execução do comando `python upload_model.py` a partir da linha de comandos. Mostra o progresso de processamento de ficheiros em 34% e o progresso de carregamento de novos dados também em 34%, e lista o progresso de carregamento dos dois ficheiros `d_model/model.safetensors` e `tokenizer_processor.safetensors`, a 92% cada. A imagem relaciona-se com o carregamento de um modelo para o HuggingFace, mostrando visualmente o progresso ao carregar ficheiros do modelo.](../../en/images/d46-05.png)

## Ver o Repositório de Modelo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![Esta imagem mostra a página do repositório HuggingFace do modelo lerobot_zihao_model_a do TommyZihao. A página mostra a Licença do modelo como mit, um contribuidor e um histórico de 2 commits. Ao centro lista vários ficheiros, como README.md, config.json e model.safetensors, cada um com o texto "Upload folder using huggingface_hub" à sua direita, indicando que estes ficheiros foram carregados através do huggingface_hub. A imagem relaciona-se com o carregamento de um modelo para o HuggingFace, mostrando visualmente como os ficheiros do modelo são armazenados no HuggingFace.](../../en/images/d46-06.png)

Agora os ficheiros do modelo estão no sítio
