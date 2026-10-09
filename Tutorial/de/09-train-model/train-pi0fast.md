[English](../en/09-train-model/train-pi0fast.md) | [简体中文](../zh-hans/09-train-model/train-pi0fast.md) | [繁體中文](../zh-hant/09-train-model/train-pi0fast.md) | Deutsch | [Español](../es/09-train-model/train-pi0fast.md) | [Français](../fr/09-train-model/train-pi0fast.md) | [Italiano](../it/09-train-model/train-pi0fast.md) | [日本語](../ja/09-train-model/train-pi0fast.md) | [한국어](../ko/09-train-model/train-pi0fast.md) | [Português (BR)](../pt-br/09-train-model/train-pi0fast.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0fast.md)

# Trainings-Kommandozeile – pi0fast

## Referenzdokumentation

https://huggingface.co/docs/lerobot/pi0fast

## Problem

https://github.com/huggingface/lerobot/pull/2203

## Empfohlene Cloud-GPU-Instanz

![Dieses Bild zeigt die Details der empfohlenen RTX-A6000-Cloud-GPU-Instanz. Es zeigt, dass 3 Karten verfügbar sind und der Pay-as-you-go-Preis 3,29 CNY/Stunde beträgt. Die Konfiguration ist eine RTX-A6000-GPU mit insgesamt 51,0 GB GPU-Speicher, einem 30-Kern-AMD-EPYC-7742-CPU, 60,9 GB Speicher und 429,5 GB Festplatte, mit einer Schaltfläche „Start Using" unten. Das Bild steht im Abschnitt „Empfohlene Cloud-GPU-Instanz" und gibt dem Nutzer die Cloud-GPU-Empfehlung samt ihrer wichtigsten Konfiguration und des Preises für das Training.](../../en/images/d41-01.png)

## Umgebung installieren

```Shell
cd lerobot
pip install -e ".[pi0]"
pip install "lerobot[pi]@git+https://github.com/huggingface/lerobot.git"
```

## Kommandozeile

- Löschen Sie die Dateien unter output, die vom zuvor unterbrochenen Trainingslauf übrig geblieben sind

```Shell
sudo rm -rf output_lerobot_train/shake/pi0_fast_A
```

- Trainieren

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





## Vorheriger Inhalt

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









Nach dem Start beginnt das Training erst nach etwa 10 Minuten richtig

Das Modellarchiv ist etwa 5 GB groß, entpackt etwa 7 GB
