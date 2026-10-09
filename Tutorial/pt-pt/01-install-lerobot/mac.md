[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | Português (PT)

# Computador Mac

O braço Leader preto utiliza um adaptador de alimentação de 5V6A.

O braço Follower branco utiliza um adaptador de alimentação de 12V5A.

## Conceder permissões

![Esta imagem mostra uma janela das definições do sistema do Mac, atualmente na página de definições de Acessibilidade, com Privacidade e Segurança selecionado na barra lateral esquerda. A janela lista várias aplicações, incluindo Baidu Netdisk, DingTalk e Doubao; o interruptor da aplicação Terminal está realçado a vermelho e encontra-se ligado. Isto corresponde ao passo "Conceder permissões" do fluxo de trabalho no Mac, ativando as permissões relevantes do Terminal em preparação para instalar o Miniconda e alterar os espelhos mais tarde.](../../en/images/d04-01.png)

## Instalar o Miniconda

https://www.anaconda.com/download

## Alterar o espelho do pip

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## Alterar o espelho do conda

```Shell
# Limpar a configuração .condarc existente (opcional, para evitar conflitos)
echo "" > ~/.condarc

# Escrever a configuração do espelho de Tsinghua
cat << EOF > ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
EOF

# Limpar a cache para que a configuração tenha efeito
conda clean -i
```

## Criar um ambiente virtual

```Shell
conda create -y -n lerobot python=3.12
```

## Ativar o ambiente virtual

```Shell
conda activate lerobot
```

## Instalar o ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

Verificar se a instalação foi bem-sucedida

```Shell
ffmpeg
```

![Esta imagem mostra a saída do terminal após executar o comando `ffmpeg`, utilizada para verificar se o ffmpeg foi instalado com êxito, correspondendo ao passo de verificação depois de "Instalar o ffmpeg". A saída mostra claramente a versão FFmpeg 7.1.1, os direitos de autor detidos pelos programadores do FFmpeg de 2000 a 2025, juntamente com as opções de configuração, a lista de codificadores suportados e as notas de utilização do Universal Media Converter, terminando com uma sugestão para utilizar a opção `-h` ou o comando `man ffmpeg` para mais ajuda.](../../en/images/d04-02.png)

## Transferir o LeRobot

- Transferir o repositório oficial do LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar o repositório de código

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![A imagem mostra a linha de comandos a instalar o pacote "feetech" com o pip num Mac. Apresenta o progresso da instalação, incluindo a obtenção de pacotes a partir de "https://repo.huaweicloud.com/repository/pypi/simple/" e a transferência de vários ficheiros, como datasets, diffusers e huggingface-hub, terminando com a transferência de "einops==0.8.0". Esta imagem está relacionada com a secção "Instalar o repositório de código", apresentando visualmente a execução do comando e o resultado da instalação do repositório.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra o ecrã de verificação num Mac após instalar o repositório de código LeRobot. O terminal apresenta "Successfully installed LeRobot" e lista vários pacotes Python instalados com as respetivas versões, como numpy e pandas. Esta imagem corresponde às secções "Instalar o repositório de código" e "Verificar a instalação", apresentando visualmente os pacotes instalados para que os utilizadores possam confirmar que a instalação foi bem-sucedida.](../../en/images/d04-04.png)
</column>
</grid>

## Verificar a instalação

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![Esta imagem é uma captura de ecrã da linha de comandos do terminal do Mac, parte da verificação da configuração nos passos de instalação do LeRobot. Mostra o diretório atual do projeto lerobot-main; após o comando python entra no ambiente interativo do Python com a versão 3.10.19 no sistema darwin. Os comandos import lerobot, import torch e torch.cuda.is_available() foram executados por ordem, e o resultado mostrou a disponibilidade de CUDA como False, seguido de import scservo_sdk. Isto corresponde ao passo de verificação, utilizado para confirmar o estado de instalação e configuração do LeRobot e das suas dependências.](../../en/images/d04-05.png)

![A imagem mostra a saída do terminal de um comando do LeRobot num Mac. Apresenta a versão LeRobot 0.4.3, a plataforma macOS - 15.6.1 - arm64 - arm - 64bit, a versão Python 3.12.12 e outras informações. Lista também informações de versão do Huggingface Hub, Datasets, NumPy, FFmpeg e PyTorch, se o PyTorch foi compilado com suporte CUDA, a versão CUDA e o modelo de GPU e, por fim, a lista de scripts do LeRobot. Esta imagem corresponde ao contexto de verificação, mostrando as informações do LeRobot após a instalação.](../../en/images/d04-06.png)
