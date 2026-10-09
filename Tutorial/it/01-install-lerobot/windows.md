[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | Italiano | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Computer Windows

Il braccio Leader nero utilizza un alimentatore da 5V6A.

Il braccio Follower bianco utilizza un alimentatore da 12V5A.

## Installare Miniconda

anaconda.com/download/success

Oppure fai clic su questo link per scaricare direttamente il programma di installazione

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![Questa immagine è la schermata di installazione di Miniconda3 su Windows, che mostra la versione del software py312_24.7.1-0 (64-bit). La schermata offre due opzioni per il tipo di installazione, in cui l'opzione etichettata "Just Me (recommended)" è evidenziata con un riquadro rosso ed è il metodo di installazione consigliato attualmente selezionato, mentre l'altra opzione, "All Users (requires admin privileges)", non è selezionata. In alto la schermata ti invita a scegliere un tipo di installazione per Miniconda3, e in basso ci sono tre pulsanti: "Back", "Next" e "Cancel". Questa schermata è il passaggio chiave del flusso di installazione di Miniconda per confermare l'ambito di installazione.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![L'immagine mostra le opzioni di installazione avanzate della schermata di installazione di Miniconda3. L'opzione "Add Miniconda3 to my PATH environment variable" è evidenziata con un riquadro rosso, con accanto una nota che spiega che questa scelta non è consigliata perché potrebbe entrare in conflitto con altre applicazioni, e che suggerisce invece i menu Prompt dei comandi e PowerShell aggiunti al menu Start di Windows. Questa immagine è collegata al passaggio di creazione di un ambiente virtuale successivo a "Modificare il mirror di conda", ed è un riferimento di configurazione durante l'installazione di Miniconda.](../../en/images/d05-02.png)
</column>
</grid>

## Modificare il mirror di conda

```Shell
# Innanzitutto svuota la configurazione del mirror esistente (per evitare conflitti)
conda config --remove-key channels

# Sostituisci i canali predefiniti di conda e i comuni canali di terze parti con il mirror Tsinghua
# Aggiungi i canali dei pacchetti predefiniti (main/r/msys2)
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# Aggiungi i comuni canali di terze parti
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# Attiva la visualizzazione dell'origine di download, così da mostrare l'indirizzo di download esatto durante l'installazione dei pacchetti
conda config --set show_channel_urls yes

# Svuota la cache dell'indice affinché i nuovi mirror abbiano effetto
conda clean -i

# Visualizza la configurazione corrente (per verificare che i canali siano stati aggiunti correttamente)
conda config --show-sources
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

![Questa immagine è la finestra della riga di comando di Windows, che mostra il risultato di verifica dopo l'esecuzione del comando ffmpeg. Nello specifico, la riga di comando restituisce la versione ffmpeg 7.1.1, con informazioni sul copyright e sulla build, insieme alle informazioni sui file di libreria associati a ffmpeg, e in basso le note d'uso che illustrano l'utilizzo di base e come ottenere ulteriore assistenza. Questa immagine serve a verificare che ffmpeg sia stato installato correttamente su un computer Windows, corrisponde al passaggio di verifica successivo a "Installare ffmpeg" e presenta lo stato di esecuzione una volta completata l'installazione di ffmpeg.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![Questa è l'interfaccia del terminale Linux, che mostra i comandi relativi a conda e la loro esecuzione. Sono chiaramente etichettati due comandi principali: il comando per attivare l'ambiente virtuale chiamato lerobot, "$ conda activate lerobot", e il comando per disattivare l'ambiente attivo, "$ conda deactivate". L'ambiente (base) è attualmente attivato, e il terminale sta eseguendo l'installazione di ffmpeg 7.1.1 dal canale conda-forge, mostrando diversi indirizzi mirror configurati, mentre il flusso di raccolta dei metadati dei pacchetti e dell'ambiente delle dipendenze è già completato.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![L'immagine mostra l'interfaccia del terminale che utilizza il comando ffmpeg in Ubuntu. Visualizza le informazioni sulla versione di ffmpeg, inclusi il numero di versione e i dettagli di configurazione del builder e del compilatore. Elenca inoltre le versioni di vari codec, come libavcodec e libavformat. Le note d'uso sono in basso, con l'invito a usare "-h" per tutta la guida, oppure a eseguire "man ffmpeg". Questa immagine è collegata alla sezione "Installare ffmpeg", usata per verificare un'installazione riuscita di ffmpeg mostrandone la versione e le informazioni di build.](../../en/images/d05-05.png)
</column>
</grid>

## Scaricare LeRobot

- Scarica il repository ufficiale LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Installare il repository di codice

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![L'immagine mostra il risultato di verifica nella riga di comando cmd di Windows dopo l'installazione del repository di codice LeRobot. La riga di comando visualizza messaggi come "Successfully built lerobot", che indicano il successo dell'installazione. Elenca inoltre diversi pacchetti Python con i relativi numeri di versione, come numpy 1.22.3 e scipy 1.7.1. In basso mostra il prompt "(lerobot) C:\\Users\\40743\\Downloads\\lerobot>", che indica che la directory corrente è la cartella lerobot sotto Downloads. Questa immagine corrisponde alla sezione "Verificare l'installazione" e presenta il riscontro della riga di comando dopo un'installazione riuscita.](../../en/images/d05-06.png)

## Verificare l'installazione

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![L'immagine mostra la schermata che verifica un'installazione riuscita nell'ambiente Python su Windows. La riga di comando visualizza la versione Python 3.10.19, ha eseguito codice che importa moduli come lerobot, scservo_sdk e torch, e infine ha eseguito torch.cuda.is_available(), che ha restituito False. Questa immagine corrisponde alla sezione "Verificare l'installazione" e presenta l'operazione e il risultato della verifica di un'installazione riuscita attraverso l'ambiente Python dopo l'installazione del repository di codice LeRobot.](../../en/images/d05-07.png)
