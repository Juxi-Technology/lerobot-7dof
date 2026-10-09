[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | Italiano | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# Riga di comando di addestramento - pi0.5

## Documentazione di riferimento

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## Istanza cloud GPU consigliata

![Questa immagine mostra i dettagli di un'istanza cloud GPU RTX A6000 offerta da Alibaba Cloud. Il suo prezzo a consumo è di 3.29 CNY/ora e sono disponibili 3 schede. L'istanza è configurata con una GPU RTX A6000 con un totale di 51.0 GB di memoria GPU, una CPU AMD EPYC 7742 a 30 core, 60.9 GB di memoria e 429.5 GB di disco. In basso c'è un pulsante blu "Start Using". L'immagine è collegata alla sezione "Istanza cloud GPU consigliata" e presenta visivamente la configurazione cloud GPU consigliata, il prezzo e altre informazioni chiave.](../../en/images/d40-01.png)

## Installare l'ambiente

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## Riga di comando

- Elimina i file sotto output lasciati dalla precedente esecuzione di addestramento interrotta

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- Addestra

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi05 \
    --output_dir=~/output_lerobot_train/shake/pi05_A \
    --job_name=shake_pi05_A \
    --policy.pretrained_path=lerobot/pi05_base \
    --policy.compile_model=true \
    --policy.gradient_checkpointing=true \
    --policy.dtype=bfloat16 \
    --policy.freeze_vision_encoder=false \
    --policy.train_expert_only=false \
    --steps=50000 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project \
    --batch_size=8
```

<grid>
<column width-ratio="0.468128">
![Questa immagine mostra l'output durante l'addestramento di un modello di physical intelligence (PI) dalla riga di comando. Mostra il caricamento del modello, la rimappatura dei parametri e la creazione dell'optimizer e dello scheduler, ad esempio "Loading model from: lerobot/pi05_base". Compaiono anche messaggi di avviso, come "Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy". Presenta inoltre valori legati all'addestramento come "num_total_frames: 180K". L'immagine è collegata all'operazione di addestramento da riga di comando e presenta visivamente le informazioni chiave dell'esecuzione.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![Questa immagine mostra l'output durante l'addestramento da riga di comando. Durante l'addestramento il processo huggingface/torch viene sottoposto a fork; poiché il parallelismo è già in uso, il parallelismo viene disattivato per evitare uno stallo, con un messaggio che consiglia di evitare di farlo "before the fork if possible". L'addestramento inizia davvero dopo 20 minuti. L'immagine è strettamente legata al contesto e presenta le operazioni di processo e i messaggi di avviso che possono comparire durante l'addestramento, aiutando a spiegare lo stato e gli aspetti a cui prestare attenzione.](../../en/images/d40-02.png)
</column>
</grid>

Dopo che la riga di comando è in esecuzione da 20 minuti, l'addestramento inizia davvero

L'archivio del modello è di circa 5 GB, e circa 7 GB una volta estratto
