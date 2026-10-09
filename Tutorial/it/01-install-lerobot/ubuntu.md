[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | Italiano | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Computer Ubuntu

Il braccio Leader nero utilizza un alimentatore da 5V6A.

Il braccio Follower bianco utilizza un alimentatore da 12V5A.

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
conda create -y -n lerobot python=3.12 -y
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

<grid>
<column width-ratio="0.568354">
![L'immagine mostra il risultato dell'attivazione di un ambiente virtuale conda e dell'installazione di ffmpeg su un computer Ubuntu. Innanzitutto conda attiva l'ambiente virtuale chiamato lerobot, poi viene eseguito il comando conda install ffmpeg=7.1.1 -c conda -forge, mostrando le informazioni sui canali di conda tra cui conda - forge, e infine la Platform linux - 64 con le operazioni completate di Collecting package metadata e Solving environment. Questa immagine corrisponde alla sezione "Installare ffmpeg" e presenta l'esecuzione dell'installazione.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![Questa immagine è uno screenshot del terminale del sistema Ubuntu, che mostra il risultato restituito dopo l'esecuzione del comando ffmpeg. Visualizza la versione ffmpeg 7.1.1, le sue informazioni di configurazione e i numeri di versione dei moduli supportati (come libavcodec e libavformat), insieme alle note d'uso del convertitore multimediale universale. Corrisponde al passaggio di verifica successivo all'installazione di ffmpeg, utilizzato per confermare che lo strumento ffmpeg è stato installato correttamente sul sistema.](../../en/images/d03-02.png)
</column>
</grid>

## Scaricare il repository ufficiale LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Installare il repository di codice

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![Questa immagine mostra il processo di accesso alla directory lerobot nel terminale del sistema Ubuntu e di esecuzione del comando `pip install -e.\[feetech\]`, che fa parte dell'installazione del repository ufficiale LeRobot. Mostra chiaramente ogni fase dell'esecuzione del comando, tra cui il recupero dei pacchetti dal repository Huawei Cloud specificato, l'installazione delle dipendenze e il download dei pacchetti di dataset correlati (come diffusers, huggingface-hub e accelerate), dove diversi pacchetti sono contrassegnati con il loro specifico avanzamento di download, dimensione e velocità, terminando con un messaggio che indica che le dipendenze sono già soddisfatte e che completa l'installazione del repository.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![Questa immagine mostra l'interfaccia a riga di comando del terminale del sistema Ubuntu durante l'installazione di pacchetti software, incluse le informazioni sulla gestione delle dipendenze durante l'installazione di software come ffmpeg. Il terminale visualizza l'elenco dei pacchetti in elaborazione, come pytz, pyyaml e numpy, nonché il processo di disinstallazione delle versioni esistenti dei pacchetti e di installazione di quelle nuove, con note sulla coerenza delle dipendenze. Questo contenuto corrisponde al passaggio "Verificare l'installazione" successivo a "Installare ffmpeg", ed è una registrazione dell'output del terminale dalla verifica del processo di installazione di ffmpeg e di altri software.](../../en/images/d03-03.png)
</column>
</grid>

## Verificare l'installazione

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## Risultati su un host 4090

![L'immagine mostra i comandi e le informazioni di LeRobot in esecuzione su un computer Ubuntu. Il comando "Lerobot lerobot -info" visualizza LeRobot versione 0.4.3, piattaforma Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, versione Python 3.12.0 e altre informazioni. In esso, la versione di PyTorch è 2.7.1 + cu126, la versione CUDA è 12.6 e il modello della GPU è NVIDIA GeForce RTX 4090. Questa immagine è collegata alla verifica di un'installazione riuscita e mostra le informazioni operative di LeRobot nell'ambiente Ubuntu.](../../en/images/d03-04.png)

![L'immagine mostra l'interfaccia interattiva di Python durante l'esecuzione del repository LeRobot su un computer Ubuntu. Visualizza la versione Python 3.10.12, incluse le informazioni sul pacchetto conda-forge e l'ora di compilazione. L'utente ha inserito in sequenza i comandi `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()` e `import scservo_sdk`, ottenendo il numero di versione di LeRobot 0.4.3, la disponibilità di CUDA True e l'importazione riuscita di `scservo_sdk`. Questa immagine è collegata alla verifica dell'installazione del repository LeRobot e presenta il processo di verifica.](../../en/images/d03-05.png)

## Risultati su un NVIDIA DGX Spark

![L'immagine mostra l'output del terminale durante l'esecuzione del repository LeRobot su un computer Ubuntu. Visualizza LeRobot versione 0.4.4, la versione CUDA 13.0 e il modello della GPU NVIDIA GeForce GTX 1660 Ti. Elenca inoltre le versioni delle librerie come HuggingFace Hub, Datasets e PyTorch, nonché le versioni degli strumenti FFmpeg e PyTorch. Infine verifica le importazioni di LeRobot e torch; torch.cuda.is_available() restituisce True, indicando che CUDA è disponibile. Questa immagine è collegata all'esecuzione del repository LeRobot su un computer Ubuntu e ne mostra i risultati.](../../en/images/d03-06.png)
