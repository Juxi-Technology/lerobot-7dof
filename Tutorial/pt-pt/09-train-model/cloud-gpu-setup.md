[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | Português (PT)

# Configurar um Ambiente de Treino com GPU na Nuvem

## Desativar o Proxy de Rede do Seu Computador

Caso contrário, poderá não conseguir abrir a linha de comandos do Jupyter

## Iniciar Sessão na Plataforma de GPU na Nuvem Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

Junte-se ao grupo de utilizadores, diga ao serviço de apoio ao cliente que é fã do Zihao, da Tongji, e receba um voucher

## Iniciar uma Instância de GPU na Nuvem

## Instalar e Configurar o Ambiente

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

## Iniciar Sessão no wandb

```Shell
wandb login
Copie e cole a API Key e, em seguida, prima Enter
```

![A imagem mostra o processo de início de sessão no W&B (Weights & Biases) com o comando `wandb login` no ambiente LeRobot. Após a execução do comando, o W&B indica para iniciar sessão em https://api.wandb.ai, refere que é possível implementar um servidor W&B local e orienta a encontrar a chave de API. De seguida, o W&B pede para colar a chave de API e premir Enter, e indica ainda que não foi encontrado nenhum ficheiro .netrc e que este está a ser criado. Por fim, o W&B mostra que a sessão foi iniciada como tommyzihao, com o estado de início de sessão em https://api.wandb.ai. Esta imagem está relacionada com o passo "Iniciar Sessão no wandb" do documento, apresentando visualmente o feedback da operação de início de sessão.](../../en/images/d35-01.png)

## Montar o Conjunto de Dados

```Shell
Copie o comando de descarregamento da instância, algo como:
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

O conjunto de dados aparece sob o diretório `~`

## Ajustar a Frequência de Guardar os Pesos (Opcional)

Abra `lerobot/src/lerobot/configs/train.py`

Altere save_freq de 20_000 para 5_000

Assim obtém os ficheiros de pesos do modelo mais cedo no treino
