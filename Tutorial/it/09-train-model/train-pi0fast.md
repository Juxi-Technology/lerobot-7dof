[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | [Deutsch](../de/09-train-model/train-pi0fast.md) | [Español](../es/09-train-model/train-pi0fast.md) | [Français](../fr/09-train-model/train-pi0fast.md) | Italiano | [日本語](../ja/09-train-model/train-pi0fast.md) | [한국어](../ko/09-train-model/train-pi0fast.md) | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# Riga di comando di addestramento - pi0fast

## Documentazione di riferimento

https://huggingface.co/docs/lerobot/pi0fast

## Problema

https://github.com/huggingface/lerobot/pull/2203

## Istanza cloud GPU consigliata

![Questa immagine mostra i dettagli dell'istanza cloud GPU RTX A6000 consigliata. Indica che sono disponibili 3 schede e che il prezzo a consumo è di 3.29 CNY/ora. La configurazione è una GPU RTX A6000 con un totale di 51.0 GB di memoria GPU, una CPU AMD EPYC 7742 a 30 core, 60.9 GB di memoria e 429.5 GB di disco, con un pulsante "Start Using" in basso. L'immagine si trova nella sezione "Istanza cloud GPU consigliata" e fornisce all'utente la raccomandazione della cloud GPU con la sua configurazione chiave e il prezzo per l'addestramento.](../../en/images/d41-01.png)

## Installare l'ambiente

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## Riga di comando

- Elimina i file sotto output lasciati dalla precedente esecuzione di addestramento interrotta

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- Addestra

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi0_fast \
    --output_dir=output_lerobot_train/shake/pi0_fast_A \
    --job_name=shake_pi0_fast_A \
    --policy.pretrained_path=lerobot/pi0_fast_base \
    --policy.dtype=bfloat16 \
    --policy.gradient_checkpointing=true \
    --policy.chunk_size=10 \
    --policy.n_action_steps=10 \
    --policy.max_action_tokens=256 \
    --steps=50000 \
    --batch_size=8 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project
```





## Contenuto precedente

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0_fast \
  --output_dir=output_lerobot_train/shake/pi0_fast_A \
  --job_name=shake_pi0_fast_A \
  --policy.pretrained_path=lerobot/pi0_fast_base \
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









Dopo l'esecuzione, l'addestramento inizia davvero dopo circa 10 minuti

L'archivio del modello è di circa 5 GB, e circa 7 GB una volta estratto
