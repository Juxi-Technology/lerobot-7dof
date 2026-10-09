[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | Português (BR) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Computador Ubuntu

O braço Leader preto usa uma fonte de alimentação de 5V6A.

O braço Follower branco usa uma fonte de alimentação de 12V5A.

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
conda create -y -n lerobot python=3.12 -y
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

<grid>
<column width-ratio="0.568354">
![A imagem mostra o resultado da ativação de um ambiente virtual conda e da instalação do ffmpeg em um computador Ubuntu. Primeiro, o conda ativa o ambiente virtual chamado lerobot; em seguida, o comando conda install ffmpeg=7.1.1 -c conda-forge é executado, mostrando as informações de Channels do conda, incluindo conda-forge, e por fim a Platform linux-64, juntamente com as operações concluídas de Collecting package metadata e Solving environment. Esta imagem corresponde à seção "Instalar o ffmpeg", apresentando visualmente a execução da instalação.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![Esta imagem é uma captura de tela do terminal do sistema Ubuntu, mostrando o resultado retornado após executar o comando ffmpeg. Ela exibe a versão 7.1.1 do ffmpeg, suas informações de configuração e os números de versão dos módulos suportados (como libavcodec e libavformat), além das instruções de uso do conversor de mídia universal. Corresponde à etapa de verificação após instalar o ffmpeg, usada para confirmar que a ferramenta ffmpeg foi instalada com sucesso no sistema.](../../en/images/d03-02.png)
</column>
</grid>

## Baixar o Repositório Oficial do LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar o Repositório de Código

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![Esta imagem mostra o processo de entrar no diretório lerobot no terminal do sistema Ubuntu e executar o comando `pip install -e.\[feetech\]`, que faz parte da instalação do repositório oficial do LeRobot. Ela mostra claramente cada etapa da execução do comando, incluindo a busca de pacotes no repositório especificado da Huawei Cloud, a instalação das dependências e o download de pacotes de conjuntos de dados relacionados (como diffusers, huggingface-hub e accelerate), em que vários pacotes são marcados com seu progresso, tamanho e velocidade de download específicos, terminando com uma mensagem de que as dependências já estão satisfeitas e concluindo a instalação do repositório.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![Esta imagem mostra a interface de linha de comando do terminal do sistema Ubuntu durante a instalação de pacotes de software, incluindo informações de tratamento de dependências ao instalar softwares como o ffmpeg. O terminal exibe a lista de pacotes sendo processados, como pytz, pyyaml e numpy, bem como o processo de desinstalação das versões existentes e instalação das novas, com observações sobre a consistência das dependências. Este conteúdo corresponde à etapa "Verificar a Instalação" após "Instalar o ffmpeg", e é um registro da saída do terminal ao verificar o processo de instalação do ffmpeg e de outros softwares.](../../en/images/d03-03.png)
</column>
</grid>

## Verificar a Instalação

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## Resultados em um Host com 4090

![A imagem mostra os comandos e as informações do LeRobot em execução em um computador Ubuntu. O comando "Lerobot lerobot -info" exibe a versão do LeRobot 0.4.3, a plataforma Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, a versão do Python 3.12.0 e outras informações. Nela, a versão do PyTorch é 2.7.1 + cu126, a versão do CUDA é 12.6 e o modelo da GPU é NVIDIA GeForce RTX 4090. Esta imagem está relacionada à verificação de uma instalação bem-sucedida, mostrando as informações de funcionamento do LeRobot no ambiente Ubuntu.](../../en/images/d03-04.png)

![A imagem mostra a interface interativa do Python durante a execução do repositório LeRobot em um computador Ubuntu. Ela exibe a versão do Python 3.10.12, incluindo informações de empacotamento do conda-forge e a data de compilação. O usuário digitou os comandos `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()` e `import scservo_sdk` em sequência, obtendo o número de versão do LeRobot 0.4.3, a disponibilidade de CUDA como True e a importação bem-sucedida de `scservo_sdk`. Esta imagem está relacionada à verificação da instalação do repositório LeRobot, apresentando visualmente o processo de verificação.](../../en/images/d03-05.png)

## Resultados em um NVIDIA DGX Spark

![A imagem mostra a saída do terminal durante a execução do repositório LeRobot em um computador Ubuntu. Ela exibe a versão do LeRobot 0.4.4, a versão do CUDA 13.0 e o modelo de GPU NVIDIA GeForce GTX 1660 Ti. Também lista versões de bibliotecas como HuggingFace Hub, Datasets e PyTorch, além das versões das ferramentas FFmpeg e PyTorch. Por fim, verifica as importações de LeRobot e torch; torch.cuda.is_available() retorna True, indicando que o CUDA está disponível. Esta imagem está relacionada à execução do repositório LeRobot em um computador Ubuntu, mostrando os resultados.](../../en/images/d03-06.png)
