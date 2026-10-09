[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | Português (PT)

# Computador Windows

O braço Leader preto utiliza um adaptador de alimentação de 5V6A.

O braço Follower branco utiliza um adaptador de alimentação de 12V5A.

## Instalar o Miniconda

anaconda.com/download/success

Ou clique nesta ligação para transferir diretamente o instalador

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![Esta imagem é o ecrã de instalação do Miniconda3 no Windows, mostrando a versão de software py312_24.7.1-0 (64-bit). O ecrã oferece duas opções de tipo de instalação, em que a opção designada "Just Me (recommended)" está realçada com um retângulo vermelho e é o método de instalação recomendado atualmente selecionado, enquanto a outra opção, "All Users (requires admin privileges)", não está selecionada. A parte superior do ecrã pede para escolher um tipo de instalação para o Miniconda3, e a parte inferior tem três botões: "Back", "Next" e "Cancel". Este ecrã é o passo essencial no fluxo de instalação do Miniconda para confirmar o âmbito da instalação.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra as opções de instalação avançadas no ecrã de instalação do Miniconda3. A opção "Add Miniconda3 to my PATH environment variable" está realçada com um retângulo vermelho, com uma nota ao lado a explicar que isto não é recomendado porque pode entrar em conflito com outras aplicações, sugerindo em alternativa os menus Command Prompt e PowerShell adicionados ao menu Iniciar do Windows. Esta imagem está relacionada com o passo de criar um ambiente virtual depois de "Alterar o espelho do conda", e é uma referência de configuração ao instalar o Miniconda.](../../en/images/d05-02.png)
</column>
</grid>

## Alterar o espelho do conda

```Shell
# Primeiro, limpar a configuração de espelho existente (para evitar conflitos)
conda config --remove-key channels

# Substituir os canais predefinidos do conda e os canais de terceiros comuns pelo espelho de Tsinghua
# Adicionar os canais de pacotes predefinidos (main/r/msys2)
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# Adicionar canais de terceiros comuns
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# Ativar a apresentação da origem da transferência, para que o endereço exato de transferência seja mostrado ao instalar pacotes
conda config --set show_channel_urls yes

# Limpar a cache do índice para que os novos espelhos tenham efeito
conda clean -i

# Ver a configuração atual (para verificar se os canais foram adicionados com êxito)
conda config --show-sources
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

![Esta imagem é a janela da linha de comandos do Windows, mostrando o resultado da verificação após executar o comando ffmpeg. Concretamente, a linha de comandos apresenta a versão ffmpeg 7.1.1, com informação de direitos de autor e de compilação, juntamente com informação sobre os ficheiros de biblioteca associados ao ffmpeg, e notas de utilização na parte inferior que abrangem a utilização básica e como obter mais ajuda. Esta imagem é utilizada para verificar que o ffmpeg foi instalado com êxito num computador Windows, correspondendo ao passo de verificação após "Instalar o ffmpeg", e apresentando visualmente o estado de funcionamento assim que a instalação do ffmpeg está concluída.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![Esta é a interface do terminal Linux, mostrando comandos relacionados com o conda e a sua execução. Dois comandos essenciais estão claramente identificados: o comando para ativar o ambiente virtual com o nome lerobot, "$ conda activate lerobot", e o comando para desativar o ambiente ativo, "$ conda deactivate". O ambiente (base) está atualmente ativado, e o terminal está a executar a instalação do ffmpeg 7.1.1 a partir do canal conda-forge, mostrando vários endereços de espelho configurados, enquanto o fluxo de recolha de metadados de pacotes e do ambiente de dependências já está concluído.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![A imagem mostra a interface do terminal a utilizar o comando ffmpeg no Ubuntu. Apresenta informação de versão do ffmpeg, incluindo o número de versão e detalhes de configuração do compilador e do construtor. Lista também as versões de vários codecs, como libavcodec e libavformat. As notas de utilização estão na parte inferior, sugerindo o uso de "-h" para toda a ajuda, ou executar "man ffmpeg". Esta imagem está relacionada com a secção "Instalar o ffmpeg", utilizada para verificar uma instalação bem-sucedida do ffmpeg mostrando a sua versão e informação de compilação.](../../en/images/d05-05.png)
</column>
</grid>

## Transferir o LeRobot

- Transferir o repositório oficial do LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar o repositório de código

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![A imagem mostra o resultado da verificação na linha de comandos cmd do Windows após instalar o repositório de código LeRobot. A linha de comandos apresenta mensagens como "Successfully built lerobot", indicando que a instalação foi bem-sucedida. Lista também vários pacotes Python e os respetivos números de versão, como numpy 1.22.3 e scipy 1.7.1. Na parte inferior mostra o prompt "(lerobot) C:\\Users\\40743\\Downloads\\lerobot>", indicando que o diretório atual é a pasta lerobot dentro de Downloads. Esta imagem corresponde à secção "Verificar a instalação", apresentando visualmente o feedback da linha de comandos após uma instalação bem-sucedida.](../../en/images/d05-06.png)

## Verificar a instalação

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![A imagem mostra o ecrã a verificar uma instalação bem-sucedida no ambiente Python no Windows. A linha de comandos apresenta a versão Python 3.10.19, executou código que importa módulos como lerobot, scservo_sdk e torch, e por fim executou torch.cuda.is_available(), que devolveu False. Esta imagem corresponde à secção "Verificar a instalação", apresentando visualmente a operação e o resultado da verificação de uma instalação bem-sucedida através do ambiente Python após instalar o repositório de código LeRobot.](../../en/images/d05-07.png)
