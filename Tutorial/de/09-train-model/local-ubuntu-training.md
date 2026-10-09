[English](../en/09-train-model/local-ubuntu-training.md) | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | Deutsch | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# Lokales Training unter Ubuntu

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- Hinweis

`\` darf nur ein Leerzeichen davor und kein Leerzeichen danach haben

`--dataset.split` ist standardmäßig `train`, das heißt, der gesamte Datensatz wird als Trainingssatz verwendet

Der Datensatz ist lokal, daher muss `--dataset.streaming` auf `false` stehen, da der Datensatz bereits auf der Festplatte liegt und kein Streaming-Lesen nötig ist

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
![Dieses Bild zeigt den Trainingsbefehl und die Konfiguration zum Ausführen des Skripts lerobot_train.py in einer lokalen Ubuntu-Umgebung. Der Befehl enthält Parameter wie den Datensatzpfad, die Repository-ID und den Branch, etwa `--dataset.repo_id` auf Tommy/lerobot_zhao_dataset_a gesetzt. Unter den Konfigurationswerten ist `--dataset.streaming` auf `false`, `--use_imagenet_stats` auf `True`, `--batch_size` auf 4 und `--val_n_episodes` auf 1000 gesetzt. Das Bild steht in engem Bezug zum Kontext und veranschaulicht den Trainingsbefehl samt seiner wichtigsten Konfigurationsparameter.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![Dieses Bild zeigt das Trainingslog, das beim Ausführen des Skripts lerobot_train.py in einer lokalen Ubuntu-Umgebung entsteht, mit den Trainingskonfigurationsparametern und dem laufenden Status des Trainings im Mittelpunkt. Es kennzeichnet deutlich zentrale Trainingsinformationen: `--dataset.split` ist standardmäßig `train`, das heißt, der gesamte Datensatz wird als Trainingssatz verwendet, und weil der Datensatz lokal gespeichert ist, ist auch der Zustand von `--dataset.streaming` entsprechend festgelegt. Das Log deckt außerdem den Trainingsfortschritt, das Laden des Datensatzes, die Erstellung von Modell-Optimizer und -Scheduler sowie die Loss- und Schrittwerte während des Trainings ab und vermittelt so einen klaren Blick auf ein laufendes lokales Training.](../../en/images/d34-02.png)
</column>
</grid>

![Dieses Bild zeigt das Trainingslog, das beim Trainieren eines Modells mit LeRobot in einer Ubuntu-Umgebung entsteht. Das Log hält Informationen des Trainingslaufs fest, etwa Uhrzeit, Trainingssatz, Modell, Loss und Genauigkeit. Die Zeitstempel reichen von 15:11:53 bis 16:15:16 am 14. Januar 2024, der Loss schwankt zwischen 0,68 und 0,65 und die Genauigkeit (acc) zwischen 0,85 und 0,88. Das Bild bezieht sich auf den LeRobot-Modelltraining-Kontext und veranschaulicht, wie sich die wichtigsten Kennzahlen während des Trainings verändern.](../../en/images/d34-03.png)
