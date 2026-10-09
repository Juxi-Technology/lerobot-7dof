[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | Italiano | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Computer Mac

Il braccio Leader nero utilizza un alimentatore da 5V6A.

Il braccio Follower bianco utilizza un alimentatore da 12V5A.

## Concedere le autorizzazioni

![L'immagine mostra la finestra delle impostazioni di sistema di Mac, attualmente sulla pagina delle impostazioni di Accessibilità, con Privacy e sicurezza selezionato nella barra laterale sinistra. La finestra elenca diverse applicazioni, tra cui Baidu Netdisk, DingTalk e Doubao; l'interruttore dell'app Terminale è evidenziato in rosso ed è attivo. Questo corrisponde al passaggio "Concedere le autorizzazioni" del flusso di lavoro su Mac, che abilita le autorizzazioni necessarie per il Terminale in preparazione all'installazione di Miniconda e alla successiva modifica dei mirror.](../../en/images/d04-01.png)

## Installare Miniconda

https://www.anaconda.com/download

## Modificare il mirror di pip

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## Modificare il mirror di conda

```Shell
# Svuota la configurazione .condarc esistente (opzionale, per evitare conflitti)
echo "" > ~/.condarc

# Scrive la configurazione del mirror Tsinghua
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

# Svuota la cache affinché la configurazione abbia effetto
conda clean -i
```

## Creare un ambiente virtuale

```Shell
conda create -y -n lerobot python=3.12
```

## Attivare l'ambiente virtuale

```Shell
conda activate lerobot
```

## Installare ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

Verificare che l'installazione sia andata a buon fine

```Shell
ffmpeg
```

![L'immagine mostra l'output del terminale dopo l'esecuzione del comando `ffmpeg`, utilizzato per verificare che ffmpeg sia stato installato correttamente; corrisponde al passaggio di verifica successivo a "Installare ffmpeg". L'output mostra chiaramente la versione FFmpeg 7.1.1, il copyright detenuto dagli sviluppatori di FFmpeg dal 2000 al 2025, insieme ai flag di configurazione, all'elenco degli encoder supportati e alle note d'uso per l'Universal Media Converter, terminando con il suggerimento di utilizzare l'opzione `-h` o il comando `man ffmpeg` per ulteriore assistenza.](../../en/images/d04-02.png)

## Scaricare LeRobot

- Scarica il repository ufficiale LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Installare il repository di codice

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![L'immagine mostra l'installazione dalla riga di comando del pacchetto "feetech" con pip su un Mac. Visualizza l'avanzamento dell'installazione, tra cui il recupero dei pacchetti da "https://repo.huaweicloud.com/repository/pypi/simple/" e il download di diversi file come datasets, diffusers e huggingface-hub, terminando con il download di "einops==0.8.0". Questa immagine è collegata alla sezione "Installare il repository di codice" e illustra l'esecuzione del comando e il risultato dell'installazione del repository.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![L'immagine mostra la schermata di verifica su un Mac dopo l'installazione del repository di codice LeRobot. Il terminale visualizza "Successfully installed LeRobot" ed elenca diversi pacchetti Python installati con le relative versioni, come numpy e pandas. Questa immagine corrisponde alle sezioni "Installare il repository di codice" e "Verificare l'installazione" e presenta i pacchetti installati così che gli utenti possano confermare il successo dell'installazione.](../../en/images/d04-04.png)
</column>
</grid>

## Verificare l'installazione

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![Questa immagine è uno screenshot della riga di comando del terminale del Mac, parte della verifica della configurazione nei passaggi di installazione di LeRobot. Mostra la directory di progetto corrente lerobot-main; dopo il comando python entra nell'ambiente Python interattivo con la versione Python 3.10.19 sul sistema darwin. I comandi import lerobot, import torch e torch.cuda.is_available() sono stati eseguiti in sequenza, e il risultato ha mostrato la disponibilità di CUDA come False, seguita da import scservo_sdk. Questo corrisponde al passaggio di verifica, utilizzato per confermare lo stato di installazione e configurazione di LeRobot e delle sue dipendenze.](../../en/images/d04-05.png)

![L'immagine mostra l'output del terminale di un comando LeRobot su un Mac. Visualizza LeRobot versione 0.4.3, piattaforma macOS - 15.6.1 - arm64 - arm - 64bit, versione Python 3.12.12 e altre informazioni. Elenca inoltre le informazioni di versione di Huggingface Hub, Datasets, NumPy, FFmpeg e PyTorch, se PyTorch è compilato con supporto CUDA, la versione CUDA e il modello della GPU, e infine l'elenco degli script di LeRobot. Questa immagine corrisponde al contesto di verifica e mostra le informazioni di LeRobot dopo l'installazione.](../../en/images/d04-06.png)
