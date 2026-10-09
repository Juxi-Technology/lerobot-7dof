[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | Português (PT)

# Computador Ubuntu

O braço Leader preto utiliza um adaptador de alimentação de 5V6A.

O braço Follower branco utiliza um adaptador de alimentação de 12V5A.

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
conda create -y -n lerobot python=3.12 -y
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

<grid>
<column width-ratio="0.568354">
![A imagem mostra o resultado da ativação de um ambiente virtual conda e da instalação do ffmpeg num computador Ubuntu. Primeiro, o conda ativa o ambiente virtual com o nome lerobot, depois é executado o comando conda install ffmpeg=7.1.1 -c conda -forge, mostrando as informações de Channels do conda, incluindo conda - forge, e por fim a Platform de linux - 64, juntamente com as operações concluídas Collecting package metadata e Solving environment. Esta imagem corresponde à secção "Instalar o ffmpeg", apresentando visualmente a execução da instalação.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![Esta imagem é uma captura de ecrã do terminal do sistema Ubuntu, mostrando o resultado devolvido após executar o comando ffmpeg. Apresenta a versão ffmpeg 7.1.1, a sua informação de configuração e os números de versão dos módulos suportados (como libavcodec e libavformat), juntamente com as notas de utilização do conversor universal de multimédia. Corresponde ao passo de verificação após instalar o ffmpeg, utilizado para confirmar que a ferramenta ffmpeg foi instalada com êxito no sistema.](../../en/images/d03-02.png)
</column>
</grid>

## Transferir o repositório oficial do LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar o repositório de código

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![Esta imagem mostra o processo de entrar no diretório lerobot no terminal do sistema Ubuntu e de executar o comando `pip install -e.\[feetech\]`, que faz parte da instalação do repositório oficial do LeRobot. Mostra claramente cada passo da execução do comando, incluindo a obtenção de pacotes a partir do repositório Huawei Cloud indicado, a instalação de dependências e a transferência de pacotes de conjuntos de dados relacionados (como diffusers, huggingface-hub e accelerate), em que vários pacotes estão marcados com o respetivo progresso de transferência, tamanho e velocidade, terminando com uma mensagem de que as dependências já estão satisfeitas e concluindo a instalação do repositório.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![Esta imagem mostra a interface da linha de comandos do terminal do sistema Ubuntu durante a instalação de pacotes de software, incluindo informação de tratamento de dependências ao instalar software como o ffmpeg. O terminal apresenta a lista de pacotes a processar, como pytz, pyyaml e numpy, bem como o processo de desinstalar as versões existentes dos pacotes e instalar as novas, com notas sobre a consistência das dependências. Este conteúdo corresponde ao passo "Verificar a instalação" após "Instalar o ffmpeg", e é um registo da saída do terminal da verificação do processo de instalação do ffmpeg e de outro software.](../../en/images/d03-03.png)
</column>
</grid>

## Verificar a instalação

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## Resultados numa máquina com 4090

![A imagem mostra os comandos e as informações do LeRobot em execução num computador Ubuntu. O comando "Lerobot lerobot -info" apresenta a versão LeRobot 0.4.3, a plataforma Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, a versão Python 3.12.0 e outras informações. Nele, a versão do PyTorch é 2.7.1 + cu126, a versão CUDA é 12.6 e o modelo de GPU é NVIDIA GeForce RTX 4090. Esta imagem está relacionada com a verificação de uma instalação bem-sucedida, mostrando as informações de funcionamento do LeRobot no ambiente Ubuntu.](../../en/images/d03-04.png)

![A imagem mostra a interface interativa do Python durante a execução do repositório LeRobot num computador Ubuntu. Apresenta a versão Python 3.10.12, incluindo informação de empacotamento conda-forge e a hora de compilação. O utilizador introduziu os comandos `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()` e `import scservo_sdk` por ordem, obtendo o número de versão do LeRobot 0.4.3, a disponibilidade de CUDA True e a importação bem-sucedida de `scservo_sdk`. Esta imagem está relacionada com a verificação da instalação do repositório LeRobot, apresentando visualmente o processo de verificação.](../../en/images/d03-05.png)

## Resultados numa NVIDIA DGX Spark

![A imagem mostra a saída do terminal durante a execução do repositório LeRobot num computador Ubuntu. Apresenta a versão LeRobot 0.4.4, a versão CUDA 13.0 e o modelo de GPU NVIDIA GeForce GTX 1660 Ti. Lista também versões de bibliotecas como HuggingFace Hub, Datasets e PyTorch, bem como versões de ferramentas para FFmpeg e PyTorch. Por fim, verifica as importações de LeRobot e torch; torch.cuda.is_available() devolve True, indicando que o CUDA está disponível. Esta imagem está relacionada com a execução do repositório LeRobot num computador Ubuntu, mostrando os resultados.](../../en/images/d03-06.png)
