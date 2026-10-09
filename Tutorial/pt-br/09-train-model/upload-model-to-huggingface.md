[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | Português (BR) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# Envio de um modelo para o HuggingFace (Opcional)

## Crie um repositório de modelo

<grid>
<column width-ratio="0.354197">
![Esta imagem mostra a interface do usuário do HuggingFace. Ela exibe um ícone de avatar; clicar nele abre um menu suspenso no qual a opção "New Model" está destacada com uma caixa vermelha. A imagem está relacionada à seção "Envio de um modelo para o HuggingFace (Opcional)" e corresponde à etapa "Crie um repositório de modelo", apresentando visualmente o ponto de entrada para criar um novo modelo no HuggingFace e ajudando o usuário a entender como criar recursos relacionados a modelos na plataforma.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![Esta imagem mostra a interface para criar um novo repositório de modelo no site do HuggingFace. O menu suspenso "Owner" está definido como "TommyZihao", o campo "Model name" contém "lerobot_zihao_model_a" e o campo "License" contém "mit". Abaixo há uma opção "Base template" e as escolhas de tipo de repositório "Public" e "Private". A imagem está relacionada à seção "Crie um repositório de modelo" e é um exemplo de preenchimento dos detalhes ao criar um repositório de modelo.](../../en/images/d46-02.png)
</column>
</grid>

## Visualize o repositório do modelo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

Ele está vazio por enquanto

![Esta imagem mostra a página do modelo "TommyZihao/lerobot_zihao_model_a" na plataforma HuggingFace. À esquerda há uma aba "Model card" para editar o cartão do modelo. À direita, a seção "Getting started with your model" explica como começar a usar o modelo, incluindo adicionar informações completas do modelo e enviar os arquivos do modelo. Abaixo, a área "Edit Model Card" permite adicionar a License, o idioma, o modelo base e outras informações do modelo. Na parte inferior, a área "Push your model files" oferece várias formas de enviar os arquivos do modelo, incluindo CLI, Python, Git, HTTPS e SSH. A imagem está relacionada ao envio de um modelo para o HuggingFace, mostrando os controles da página.](../../en/images/d46-03.png)

![Esta imagem mostra a página do repositório do modelo TommyZihao/lerobot_zihao_model_a na plataforma HuggingFace. A página mostra o tamanho do arquivo do modelo como 1.54 KB, um contribuidor e um histórico de 1 commit, feito 9 minutos atrás. Ela também lista os arquivos .gitattributes e README.md, com tamanhos de 1.52 KB e 24 Bytes respectivamente, igualmente do commit inicial, também 9 minutos atrás. A imagem está relacionada ao envio de um modelo para o HuggingFace, mostrando como a página fica após o envio do modelo.](../../en/images/d46-04.png)

## Envie o modelo

Crie um arquivo `upload_model.py` com o seguinte conteúdo

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

![Esta imagem mostra a saída do comando `python upload_model.py` executado pela linha de comando. Ela mostra o progresso de processamento de arquivos em 34% e o progresso de envio dos novos dados também em 34%, e lista o progresso de envio dos dois arquivos `d_model/model.safetensors` e `tokenizer_processor.safetensors` em 92% cada. A imagem está relacionada ao envio de um modelo para o HuggingFace, mostrando visualmente o progresso ao enviar os arquivos do modelo.](../../en/images/d46-05.png)

## Visualize o repositório do modelo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![Esta imagem mostra a página do repositório do HuggingFace do modelo lerobot_zihao_model_a de TommyZihao. A página mostra a License do modelo como mit, um contribuidor e um histórico de 2 commits. No meio ela lista vários arquivos, como README.md, config.json e model.safetensors, cada um com o texto "Upload folder using huggingface_hub" à direita, indicando que esses arquivos foram enviados via huggingface_hub. A imagem está relacionada ao envio de um modelo para o HuggingFace, mostrando visualmente como os arquivos do modelo são armazenados no HuggingFace.](../../en/images/d46-06.png)

Agora os arquivos do modelo estão prontos
