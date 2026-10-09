[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | [Deutsch](../de/09-train-model/train-pi0.md) | [Español](../es/09-train-model/train-pi0.md) | [Français](../fr/09-train-model/train-pi0.md) | Italiano | [日本語](../ja/09-train-model/train-pi0.md) | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# Riga di comando di addestramento - pi0 (risultati migliori)

## Documentazione di riferimento

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## Istanza cloud GPU consigliata

![Questa immagine mostra i dettagli di un'istanza cloud GPU RTX A6000. Il suo prezzo a consumo è di 3.29 CNY/ora; la GPU è una RTX A6000 con un totale di 51.0 GB di memoria GPU; la CPU è un AMD EPYC 7742 a 30 core; la memoria è di 60.9 GB e il disco di 429.5 GB. Nell'angolo in alto a destra è indicato che sono disponibili 3 schede. In basso c'è un pulsante blu "Start Using". L'immagine è collegata alla sezione "Istanza cloud GPU consigliata" e presenta visivamente la configurazione cloud GPU consigliata, il prezzo e altre informazioni chiave.](../../en/images/d39-01.png)

## Installare l'ambiente

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## Riga di comando

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_A

lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=pi0 \
  --output_dir=~/output_lerobot_train/shake/pi0_A \
  --job_name=shake_pi0_A \
  --policy.pretrained_path=lerobot/pi0_base \
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

Dopo che la riga di comando è in esecuzione da 20 minuti, l'addestramento inizia davvero

L'archivio del modello è di circa 5 GB, e circa 7 GB una volta estratto
