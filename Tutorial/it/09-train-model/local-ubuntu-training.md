[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | Italiano | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# Addestramento locale su Ubuntu

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- Nota

`\` può avere solo uno spazio prima e nessuno spazio dopo

`--dataset.split` ha come valore predefinito `train`, il che significa che l'intero dataset viene usato come insieme di addestramento

Il dataset è locale, quindi `--dataset.streaming` deve essere `false`, poiché il dataset è già su disco e non serve una lettura in streaming

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8

lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![Questa immagine mostra il comando e la configurazione di addestramento per eseguire lo script lerobot_train.py in un ambiente Ubuntu locale. Il comando include parametri come il percorso del dataset, l'ID del repository e il branch, ad esempio `--dataset.repo_id` impostato su Tommy/lerobot_zhao_dataset_a. Tra i valori di configurazione, `--dataset.streaming` è impostato su `false`, `--use_imagenet_stats` su `True`, `--batch_size` su 4 e `--val_n_episodes` su 1000. L'immagine è strettamente legata al contesto e presenta visivamente il comando di addestramento e i suoi parametri di configurazione principali.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![Questa immagine mostra il log di addestramento prodotto eseguendo lo script lerobot_train.py in un ambiente Ubuntu locale, incentrato sui parametri di configurazione dell'addestramento e sullo stato in tempo reale dell'esecuzione. Evidenzia chiaramente informazioni chiave: `--dataset.split` ha come valore predefinito `train`, quindi l'intero dataset viene usato come insieme di addestramento, e poiché il dataset è memorizzato localmente anche lo stato di `--dataset.streaming` è fissato di conseguenza. Il log riporta inoltre l'avanzamento dell'addestramento, il caricamento del dataset, la creazione dell'optimizer e dello scheduler del modello, e i valori di loss e step durante l'addestramento, offrendo una visione chiara di un addestramento locale in corso.](../../en/images/d34-02.png)
</column>
</grid>

![Questa immagine mostra il log di addestramento prodotto durante l'addestramento di un modello con LeRobot in un ambiente Ubuntu. Il log registra informazioni dell'esecuzione, come l'orario, l'insieme di addestramento, il modello, la loss e l'accuratezza. I timestamp vanno dalle 15:11:53 alle 16:15:16 del 14 gennaio 2024, la loss oscilla tra 0.68 e 0.65 e l'accuratezza (acc) tra 0.85 e 0.88. L'immagine è collegata al contesto dell'addestramento del modello LeRobot e presenta visivamente come cambiano le metriche principali durante l'addestramento.](../../en/images/d34-03.png)
