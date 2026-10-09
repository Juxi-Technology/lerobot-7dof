[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | Italiano | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# Configurare un ambiente di addestramento su cloud GPU

## Disattivare il proxy di rete del computer

Altrimenti potresti non riuscire ad aprire la riga di comando di Jupyter

## Accedere alla piattaforma cloud GPU Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

Iscriviti al gruppo utenti e di' al servizio clienti che sei un fan di Zihao (同济子豪兄) per ritirare il voucher

## Avviare un'istanza cloud GPU

## Installare e configurare l'ambiente

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## Accedere a wandb

```Shell
wandb login
Copia e incolla la API Key, premi Invio
```

![L'immagine mostra il processo di accesso a W&B (Weights & Biases) con il comando `wandb login` nell'ambiente Lerobot. Dopo l'esecuzione del comando, W&B propone di accedere a https://api.wandb.ai, spiega che è possibile distribuire un server W&B locale e guida alla ricerca della chiave API. Poi W&B chiede di incollare la chiave API e premere Invio, e segnala che il file .netrc non è stato trovato e che ne sta creando uno. Infine W&B mostra che l'accesso è avvenuto come tommyzihao, con stato di connessione a https://api.wandb.ai. Questa immagine è collegata al passaggio "Accedere a wandb" del documento e mostra visivamente il riscontro dell'operazione di accesso.](../../en/images/d35-01.png)

## Montare il dataset

```Shell
Copia il comando di download dell'istanza, qualcosa come:
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

Il dataset compare nella directory `~`

## Regolare la frequenza di salvataggio dei pesi (facoltativo)

Apri `lerobot/src/lerobot/configs/train.py`

Cambia save_freq da 20_000 a 5_000

In questo modo ottieni prima i file dei pesi del modello durante l'addestramento
