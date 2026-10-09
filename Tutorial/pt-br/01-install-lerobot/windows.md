[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | Português (BR) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Computador Windows

O braço Leader preto usa uma fonte de alimentação de 5V6A.

O braço Follower branco usa uma fonte de alimentação de 12V5A.

## Instalar o Miniconda

anaconda.com/download/success

Ou clique neste link para baixar o instalador diretamente

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![Esta imagem é a tela de instalação do Miniconda3 no Windows, mostrando a versão do software py312_24.7.1-0 (64 bits). A tela oferece duas opções de tipo de instalação, nas quais a opção rotulada "Just Me (recommended)" está destacada com uma caixa vermelha e é o método de instalação recomendado selecionado no momento, enquanto a outra opção, "All Users (requires admin privileges)", não está selecionada. No topo da tela há uma solicitação para escolher o tipo de instalação do Miniconda3, e na parte inferior há três botões: "Back", "Next" e "Cancel". Esta tela é a etapa-chave do fluxo de instalação do Miniconda para confirmar o escopo da instalação.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra as opções avançadas de instalação na tela do Miniconda3. A opção "Add Miniconda3 to my PATH environment variable" está destacada com uma caixa vermelha, com uma observação ao lado explicando que isso não é recomendado porque pode entrar em conflito com outros aplicativos, e sugerindo, em vez disso, os menus Command Prompt e PowerShell adicionados ao menu Iniciar do Windows. Esta imagem está relacionada à etapa de criação de um ambiente virtual após "Alterar o espelho do conda", e é uma referência de configuração ao instalar o Miniconda.](../../en/images/d05-02.png)
</column>
</grid>

## Alterar o espelho do conda

```Shell
# Primeiro limpa a configuração de espelho existente (para evitar conflitos)
conda config --remove-key channels

# Substitui os canais padrão do conda e os canais de terceiros comuns pelo espelho da Tsinghua
# Adiciona os canais de pacotes padrão (main/r/msys2)
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# Adiciona os canais de terceiros comuns
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# Ativa a exibição da origem do download, para que o endereço exato de download seja mostrado ao instalar pacotes
conda config --set show_channel_urls yes

# Limpa o cache de índices para que os novos espelhos tenham efeito
conda clean -i

# Visualiza a configuração atual (para verificar se os canais foram adicionados com sucesso)
conda config --show-sources
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

![Esta imagem é a janela de linha de comando do Windows, mostrando o resultado da verificação após executar o comando ffmpeg. Especificamente, a linha de comando exibe a versão 7.1.1 do ffmpeg, com informações de direitos autorais e de compilação, juntamente com informações sobre os arquivos de biblioteca associados ao ffmpeg, e instruções de uso na parte inferior cobrindo o uso básico e como obter mais ajuda. Esta imagem é usada para verificar se o ffmpeg foi instalado com sucesso em um computador Windows, correspondendo à etapa de verificação após "Instalar o ffmpeg", e apresentando visualmente o estado de execução quando a instalação do ffmpeg é concluída.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![Esta é a interface do terminal Linux, mostrando comandos relacionados ao conda e sua execução. Dois comandos principais são claramente rotulados: o comando para ativar o ambiente virtual chamado lerobot, "$ conda activate lerobot", e o comando para desativar o ambiente ativo, "$ conda deactivate". O ambiente (base) está ativado no momento, e o terminal está executando a instalação do ffmpeg 7.1.1 a partir do canal conda-forge, mostrando vários endereços de espelho configurados, enquanto o fluxo de coleta de metadados dos pacotes e do ambiente de dependências já foi concluído.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![A imagem mostra a interface do terminal usando o comando ffmpeg no Ubuntu. Ela exibe as informações de versão do ffmpeg, incluindo o número de versão, o compilador e os detalhes da configuração de compilação. Também lista as versões de vários codecs, como libavcodec e libavformat. As instruções de uso estão na parte inferior, sugerindo usar "-h" para toda a ajuda, ou executar "man ffmpeg". Esta imagem está relacionada à seção "Instalar o ffmpeg", usada para verificar uma instalação bem-sucedida do ffmpeg mostrando suas informações de versão e compilação.](../../en/images/d05-05.png)
</column>
</grid>

## Baixar o LeRobot

- Baixe o repositório oficial do LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar o Repositório de Código

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![A imagem mostra o resultado da verificação na linha de comando do cmd do Windows após instalar o repositório de código do LeRobot. A linha de comando exibe mensagens como "Successfully built lerobot", indicando que a instalação foi bem-sucedida. Também lista vários pacotes Python e seus números de versão, como numpy 1.22.3 e scipy 1.7.1. Na parte inferior é exibido o prompt "(lerobot) C:\Users\40743\Downloads\lerobot>", indicando que o diretório atual é a pasta lerobot dentro de Downloads. Esta imagem corresponde à seção "Verificar a Instalação", apresentando visualmente o retorno da linha de comando após uma instalação bem-sucedida.](../../en/images/d05-06.png)

## Verificar a Instalação

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![A imagem mostra a tela que verifica uma instalação bem-sucedida no ambiente Python no Windows. A linha de comando exibe a versão do Python 3.10.19, executou um código que importa módulos como lerobot, scservo_sdk e torch, e por fim executou torch.cuda.is_available(), que retornou False. Esta imagem corresponde à seção "Verificar a Instalação", apresentando visualmente a operação e o resultado da verificação de uma instalação bem-sucedida por meio do ambiente Python após instalar o repositório de código do LeRobot.](../../en/images/d05-07.png)
