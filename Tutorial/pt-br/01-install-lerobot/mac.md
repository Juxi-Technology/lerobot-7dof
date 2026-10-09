[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | Português (BR) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Computador Mac

O braço Leader preto usa uma fonte de alimentação de 5V6A.

O braço Follower branco usa uma fonte de alimentação de 12V5A.

## Conceder Permissões

![Esta imagem mostra uma janela de configurações do sistema do Mac, atualmente na página de acessibilidade (Accessibility), com Privacidade e Segurança (Privacy & Security) selecionada na barra lateral esquerda. A janela lista vários aplicativos, incluindo Baidu Netdisk, DingTalk e Doubao; o interruptor do aplicativo Terminal está destacado em vermelho e está ativado. Isso corresponde à etapa "Conceder Permissões" do fluxo de trabalho no Mac, habilitando as permissões relevantes do Terminal como preparação para instalar o Miniconda e alterar os espelhos mais adiante.](../../en/images/d04-01.png)

## Instalar o Miniconda

https://www.anaconda.com/download

## Alterar o espelho do pip

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## Alterar o espelho do conda

```Shell
# Limpa a configuração .condarc existente (opcional, para evitar conflitos)
echo "" > ~/.condarc

# Escreve a configuração do espelho da Tsinghua
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

# Limpa o cache para que a configuração tenha efeito
conda clean -i
```

## Criar um Ambiente Virtual

```Shell
conda create -y -n lerobot python=3.12
```

## Ativar o Ambiente Virtual

```Shell
conda activate lerobot
```

## Instalar o ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

Verifique se a instalação foi bem-sucedida

```Shell
ffmpeg
```

![Esta imagem mostra a saída do terminal após executar o comando `ffmpeg`, usada para verificar se o ffmpeg foi instalado com sucesso, correspondendo à etapa de verificação após "Instalar o ffmpeg". A saída mostra claramente o FFmpeg versão 7.1.1, com direitos autorais dos desenvolvedores do FFmpeg de 2000 a 2025, além das flags de configuração, a lista de codificadores suportados e as instruções de uso do Universal Media Converter, terminando com uma dica para usar a opção `-h` ou o comando `man ffmpeg` para obter mais ajuda.](../../en/images/d04-02.png)

## Baixar o LeRobot

- Baixe o repositório oficial do LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar o Repositório de Código

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![A imagem mostra a linha de comando instalando o pacote "feetech" com o pip em um Mac. Ela exibe o progresso da instalação, incluindo a busca de pacotes em "https://repo.huaweicloud.com/repository/pypi/simple/" e o download de vários arquivos como datasets, diffusers e huggingface-hub, terminando com o download de "einops==0.8.0". Esta imagem está relacionada à seção "Instalar o Repositório de Código", apresentando visualmente a execução do comando e o resultado ao instalar o repositório.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra a tela de verificação no Mac após instalar o repositório de código do LeRobot. O terminal exibe "Successfully installed LeRobot" e lista vários pacotes Python instalados com suas versões, como numpy e pandas. Esta imagem corresponde às seções "Instalar o Repositório de Código" e "Verificar a Instalação", apresentando visualmente os pacotes instalados para que os usuários possam confirmar que a instalação foi bem-sucedida.](../../en/images/d04-04.png)
</column>
</grid>

## Verificar a Instalação

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![Esta imagem é uma captura de tela da linha de comando do terminal do Mac, parte da verificação da configuração nas etapas de instalação do LeRobot. Ela mostra o diretório atual do projeto lerobot-main; após o comando python, entra no ambiente interativo do Python com a versão 3.10.19 no sistema darwin. Os comandos import lerobot, import torch e torch.cuda.is_available() foram executados em sequência, e o resultado mostrou a disponibilidade de CUDA como False, seguido por import scservo_sdk. Isso corresponde à etapa de verificação, usada para confirmar o status de instalação e configuração do LeRobot e suas dependências.](../../en/images/d04-05.png)

![A imagem mostra a saída do terminal de um comando do LeRobot em um Mac. Ela exibe a versão do LeRobot 0.4.3, a plataforma macOS - 15.6.1 - arm64 - arm - 64bit, a versão do Python 3.12.12 e outras informações. Também lista informações de versão do Huggingface Hub, Datasets, NumPy, FFmpeg e PyTorch, se o PyTorch foi compilado com suporte a CUDA, a versão do CUDA e o modelo da GPU, e por fim a lista de scripts do LeRobot. Esta imagem corresponde ao contexto de verificação, mostrando as informações do LeRobot após a instalação.](../../en/images/d04-06.png)
