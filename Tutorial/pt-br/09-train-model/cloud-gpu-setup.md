[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | Português (BR) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# Configuração de um Ambiente de Treinamento com GPU em Nuvem

## Desative o Proxy de Rede do Seu Computador

Caso contrário, você pode não conseguir abrir a linha de comando do Jupyter

## Faça Login na Plataforma de GPU em Nuvem Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

Entre no grupo de usuários, diga ao atendimento que você é fã do Zihao da Tongji e receba um cupom de desconto

## Inicie uma Instância de GPU em Nuvem

## Instale e Configure o Ambiente

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## Faça Login no wandb

```Shell
wandb login
Copie e cole a API Key, pressione Enter
```

![A imagem mostra o processo de login no W&B (Weights & Biases) usando o comando `wandb login` no ambiente LeRobot. Após a execução do comando, o W&B pede que você faça login em https://api.wandb.ai, informa que você pode implantar um servidor W&B local e orienta a encontrar a chave de API. Em seguida, o W&B solicita que você cole a chave de API e pressione Enter, e também indica que nenhum arquivo .netrc foi encontrado e que ele está sendo criado. Por fim, o W&B mostra que você está conectado como tommyzihao, com o status de login https://api.wandb.ai. Esta imagem está relacionada à etapa "Faça Login no wandb" do documento, apresentando visualmente o retorno da operação de login.](../../en/images/d35-01.png)

## Monte o Conjunto de Dados

```Shell
Copie o comando de download da instância, algo como:
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

O conjunto de dados aparece no diretório `~`

## Ajustar a Frequência de Salvamento dos Pesos (Opcional)

Abra `lerobot/src/lerobot/configs/train.py`

Altere save_freq de 20_000 para 5_000

Assim você obtém os arquivos de peso do modelo mais cedo durante o treinamento
