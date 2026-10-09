[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | Italiano | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>Caricare il dataset su HuggingFace (opzionale)</title>

# Metodo 1: caricare localmente (non consigliato; velocità di caricamento lenta)

- Caricamento automatico

Imposta `push_to_hub=true` durante la raccolta del dataset, e questo verrà caricato automaticamente una volta terminata la raccolta

![Questa immagine mostra un'interfaccia a riga di comando, parte del log di esecuzione del processo di caricamento del dataset. In alto mostra le informazioni sull'ambiente per strumenti come SVN e treet W2; al centro c'è un messaggio di elaborazione come "Starting the second pass: moving the mov atom to the beginning of the file", e in basso ci sono errori dell'esecuzione come "error messaging the mach port for IMCRunLoopWakeUpReliable", mentre a destra elenca i progressi di elaborazione e le cifre di trasferimento dei dati, come la quantità di dati e la velocità per le diverse voci. Nel complesso presenta un registro dello stato di esecuzione durante l'elaborazione del caricamento del dataset.](../../en/images/d28-01.png)

- Caricamento manuale

Imposta `push_to_hub=false` durante la raccolta del dataset, e caricalo manualmente una volta terminata la raccolta

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



Sia che si tratti di caricamento automatico o manuale, la velocità di caricamento è molto lenta (circa 100 KB al secondo)

perché i server di HuggingFace sono all'estero

# Metodo 2: caricare da una piattaforma GPU cloud (consigliato)

## Accedere alla piattaforma GPU cloud Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## Avviare un'istanza GPU cloud

## Caricare l'archivio del dataset in `Datasets`

## Copiare il comando di download dell'istanza

![L'immagine mostra la pagina datasets della piattaforma Featurize. In alto mostra il titolo "Datasets", e sotto c'è un dataset chiamato "soarm_amazing_hand_pick.zip", di 213.3 MB, caricato 16 ore fa. A destra c'è un pulsante "Cloud Unzip", insieme a pulsanti come "Like", "Comment" e "Copy Instance Download Command". Questa immagine è collegata al passaggio "Caricare l'archivio del dataset in `Datasets`" e mostra la pagina datasets dopo il caricamento.](../../en/images/d28-02.png)

## Eseguire nella riga di comando dell'istanza GPU cloud

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## Caricare il dataset su HuggingFace

Crea un file `upload_dataset.py` con il seguente contenuto

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

Esegui il file

```Shell
python upload_dataset.py
```

![Questa immagine mostra il processo di esecuzione del caricamento del dataset nella riga di comando di un'istanza GPU cloud, dove un utente di nome lerobot2 ha eseguito il comando python upload.py. Mostra i progressi di elaborazione dei file, con 6 file da elaborare e tutti al 100% di avanzamento, e contrassegna la dimensione di trasferimento di ciascun file, con i progressi totali di trasferimento dei dati al 100%. In basso nota che nessun file è stato modificato dall'ultimo commit, quindi il commit viene saltato per evitare di creare un commit vuoto. Questo contenuto corrisponde al passaggio di esecuzione del file upload_dataset.py.](../../en/images/d28-03.png)

- Un altro metodo di caricamento (non consigliato)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![Questa immagine mostra il processo di caricamento di un dataset tramite il comando `hf upload` di HuggingFace, con il comando `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`. L'immagine mostra che il caricamento è entrato nella fase finale, con tutti i file al 100% di avanzamento, tra cui diversi file video e file parquet le cui dimensioni di caricamento corrispondono esattamente alle dimensioni dei file locali corrispondenti, insieme alla dimensione totale dei file caricati e alla velocità di trasferimento, e in basso un link alla pagina del dataset HuggingFace per questo commit di caricamento, che indica che l'attività di caricamento è completata.](../../en/images/d28-04.png)

# Visualizzare il dataset su HuggingFace

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![Questa immagine è uno screenshot della pagina dei dettagli del dataset soarm_amazing_hand_pick del team Juxi-Technology sulla piattaforma Hugging Face, corrispondente al contenuto "Visualizzare il dataset su HuggingFace". In alto mostra le opzioni di navigazione per il dataset e le informazioni di base come autore e tag; al centro l'area Dataset Viewer mostra parte dei dati di addestramento per lo split 1 del dataset, inclusi campi come action, observation_state e timestamp e i loro valori, e contrassegna anche la dimensione di un singolo record, il numero totale di record e la dimensione totale. In basso menziona anche i modelli correlati addestrati su questi dati.](../../en/images/d28-05.png)

![L'immagine mostra la pagina del dataset soarm_amazing_hand_pick sulla piattaforma Hugging Face. In alto c'è una casella di ricerca e una barra di navigazione per cercare modelli, dataset e così via. La sezione delle informazioni sul dataset mostra l'organizzazione proprietaria Juxi - Technology e tag come robotics e imitation-learning. Sotto la scheda "Files and versions" elenca cartelle come data, meta e videos e il file README.md, mostrando chi ha caricato, il metodo di caricamento e l'ora, come "Upload README.md with huggingface_hub". Questa immagine è collegata alla visualizzazione di un dataset HuggingFace e presenta i file e le versioni del dataset.](../../en/images/d28-06.png)
