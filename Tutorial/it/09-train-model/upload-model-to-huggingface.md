[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | Italiano | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# Caricare un modello su HuggingFace (facoltativo)

## Creare un repo per il modello

<grid>
<column width-ratio="0.354197">
![Questa immagine mostra l'interfaccia utente di HuggingFace. È visibile un'icona dell'avatar; facendovi clic si apre un menu a tendina in cui l'opzione "New Model" è evidenziata da un riquadro rosso. L'immagine è collegata alla sezione "Caricare un modello su HuggingFace (facoltativo)" e corrisponde al passaggio "Creare un repo per il modello", presentando visivamente il punto di accesso per creare un nuovo modello su HuggingFace e aiutando l'utente a capire come creare le risorse relative al modello sulla piattaforma.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![Questa immagine mostra l'interfaccia per creare un nuovo repository di modello sul sito di HuggingFace. Il menu a tendina "Owner" è impostato su "TommyZihao", il campo "Model name" contiene "lerobot_zihao_model_a" e il campo "License" contiene "mit". Sotto ci sono un'opzione "Base template" e le scelte di tipo di repository "Public" e "Private". L'immagine è collegata alla sezione "Creare un repo per il modello" ed è un esempio di compilazione dei dati durante la creazione di un repository di modello.](../../en/images/d46-02.png)
</column>
</grid>

## Visualizzare il repo del modello

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

Attualmente è vuoto

![Questa immagine mostra la pagina del modello "TommyZihao/lerobot_zihao_model_a" sulla piattaforma HuggingFace. A sinistra c'è una scheda "Model card" per modificare la model card. A destra, la sezione "Getting started with your model" spiega come iniziare a usare il modello, inclusi l'aggiunta delle informazioni complete del modello e il push dei file del modello. Sotto, l'area "Edit Model Card" consente di aggiungere la License, la lingua, il modello di base e altre informazioni del modello. In fondo, l'area "Push your model files" offre diversi modi per caricare i file del modello, tra cui CLI, Python, Git, HTTPS e SSH. L'immagine è collegata al caricamento di un modello su HuggingFace e mostra i controlli della pagina.](../../en/images/d46-03.png)

![Questa immagine mostra la pagina del repo del modello TommyZihao/lerobot_zihao_model_a sulla piattaforma HuggingFace. La pagina mostra la dimensione dei file del modello pari a 1.54 KB, un contributore e uno storico di 1 commit, effettuato 9 minuti fa. Elenca inoltre i file .gitattributes e README.md, rispettivamente di 1.52 KB e 24 Bytes, anch'essi del commit iniziale, sempre di 9 minuti fa. L'immagine è collegata al caricamento di un modello su HuggingFace e mostra come appare la pagina dopo il caricamento del modello.](../../en/images/d46-04.png)

## Caricare il modello

Crea un file `upload_model.py` con il seguente contenuto

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

Eseguilo

```Shell
python upload_model.py
```

![Questa immagine mostra l'output dell'esecuzione del comando `python upload_model.py` dalla riga di comando. Mostra l'avanzamento dell'elaborazione dei file al 34% e l'avanzamento del caricamento dei nuovi dati anch'esso al 34%, ed elenca l'avanzamento del caricamento dei due file `d_model/model.safetensors` e `tokenizer_processor.safetensors`, entrambi al 92%. L'immagine è collegata al caricamento di un modello su HuggingFace e mostra visivamente l'avanzamento durante il caricamento dei file del modello.](../../en/images/d46-05.png)

## Visualizzare il repo del modello

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![Questa immagine mostra la pagina del repo HuggingFace del modello lerobot_zihao_model_a di TommyZihao. La pagina mostra la License del modello come mit, un contributore e uno storico di 2 commit. Al centro elenca diversi file, come README.md, config.json e model.safetensors, ognuno con il testo "Upload folder using huggingface_hub" alla sua destra, a indicare che questi file sono stati caricati tramite huggingface_hub. L'immagine è collegata al caricamento di un modello su HuggingFace e mostra visivamente come i file del modello sono archiviati su HuggingFace.](../../en/images/d46-06.png)

Ora i file del modello sono al loro posto
