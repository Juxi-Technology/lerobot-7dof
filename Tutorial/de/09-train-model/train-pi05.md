[English](../en/09-train-model/train-pi05.md) | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | Deutsch | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# Trainings-Kommandozeile – pi0.5

## Referenzdokumentation

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## Empfohlene Cloud-GPU-Instanz

![Dieses Bild zeigt die Details einer von Alibaba Cloud angebotenen RTX-A6000-Cloud-GPU-Instanz. Ihr Pay-as-you-go-Preis beträgt 3,29 CNY/Stunde, und 3 Karten sind verfügbar. Die Instanz ist mit einer RTX-A6000-GPU mit insgesamt 51,0 GB GPU-Speicher, einem 30-Kern-AMD-EPYC-7742-CPU, 60,9 GB Speicher und 429,5 GB Festplatte ausgestattet. Unten befindet sich eine blaue Schaltfläche „Start Using". Das Bild bezieht sich auf den Abschnitt „Empfohlene Cloud-GPU-Instanz" und veranschaulicht die empfohlene Cloud-GPU-Konfiguration, den Preis und weitere wichtige Informationen.](../../en/images/d40-01.png)

## Umgebung installieren

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## Kommandozeile

- Löschen Sie die Dateien unter output, die vom zuvor unterbrochenen Trainingslauf übrig geblieben sind

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- Trainieren

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
![Dieses Bild zeigt die Ausgabe beim Trainieren eines Modells der physischen Intelligenz (PI) über die Kommandozeile. Es zeigt, wie das Modell geladen wird, Parameter neu zugeordnet werden und der Optimizer sowie der Scheduler erstellt werden, zum Beispiel „Loading model from: lerobot/pi05_base". Außerdem erscheinen Warnmeldungen, etwa „Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy". Es zeigt außerdem trainingsbezogene Zahlen wie „num_total_frames: 180K". Das Bild bezieht sich auf den Vorgang der Trainings-Kommandozeile und veranschaulicht die wichtigsten Informationen des Trainingslaufs.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![Dieses Bild zeigt die Ausgabe während des Trainings über die Kommandozeile. Während des Trainings wird der huggingface/torch-Prozess geforkt; da bereits Parallelität genutzt wird, wird die Parallelität deaktiviert, um einen Deadlock zu vermeiden, mit einer Meldung, dies möglichst nicht „before the fork if possible" zu tun. Das Training beginnt erst nach 20 Minuten richtig. Das Bild steht in engem Bezug zum Kontext, zeigt die Prozessvorgänge und Hinweismeldungen, die während des Trainings auftreten können, und hilft, den Zustand und die Punkte zu erläutern, auf die zu achten ist.](../../en/images/d40-02.png)
</column>
</grid>

Erst nachdem die Kommandozeile 20 Minuten lang läuft, beginnt das Training richtig

Das Modellarchiv ist etwa 5 GB groß, entpackt etwa 7 GB
