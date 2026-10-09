[English](../en/09-train-model/train-pi0.md) | [简体中文](../zh-hans/09-train-model/train-pi0.md) | [繁體中文](../zh-hant/09-train-model/train-pi0.md) | Deutsch | [Español](../es/09-train-model/train-pi0.md) | [Français](../fr/09-train-model/train-pi0.md) | [Italiano](../it/09-train-model/train-pi0.md) | [日本語](../ja/09-train-model/train-pi0.md) | [한국어](../ko/09-train-model/train-pi0.md) | [Português (BR)](../pt-br/09-train-model/train-pi0.md) | [Português (PT)](../pt-pt/09-train-model/train-pi0.md)

# Trainings-Kommandozeile – pi0 (beste Ergebnisse)

## Referenzdokumentation

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

## Empfohlene Cloud-GPU-Instanz

![Dieses Bild zeigt die Details einer RTX-A6000-Cloud-GPU-Instanz. Ihr Pay-as-you-go-Preis beträgt 3,29 CNY/Stunde; die GPU ist eine RTX A6000 mit insgesamt 51,0 GB GPU-Speicher; die CPU ist ein 30-Kern-AMD-EPYC-7742; der Speicher beträgt 60,9 GB; und die Festplatte 429,5 GB. In der oberen rechten Ecke ist zu sehen, dass 3 Karten verfügbar sind. Unten befindet sich eine blaue Schaltfläche „Start Using". Das Bild bezieht sich auf den Abschnitt „Empfohlene Cloud-GPU-Instanz" und veranschaulicht die empfohlene Cloud-GPU-Konfiguration, den Preis und weitere wichtige Informationen.](../../en/images/d39-01.png)

## Umgebung installieren

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip install -e ".[pi]"
```

## Kommandozeile

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

Erst nachdem die Kommandozeile 20 Minuten lang läuft, beginnt das Training richtig

Das Modellarchiv ist etwa 5 GB groß, entpackt etwa 7 GB
