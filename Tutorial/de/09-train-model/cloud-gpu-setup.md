[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | Deutsch | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# Einrichten einer Cloud-GPU-Trainingsumgebung

## Netzwerk-Proxy des eigenen Computers deaktivieren

Andernfalls lässt sich die Jupyter-Kommandozeile möglicherweise nicht öffnen

## Bei der Cloud-GPU-Plattform Featurize anmelden

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

Treten Sie der Nutzergruppe bei, sagen Sie dem Kundenservice, dass Sie ein Fan von Zihao aus Tongji sind, und holen Sie sich einen Gutschein

## Eine Cloud-GPU-Instanz starten

## Umgebung installieren und konfigurieren

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

## Bei wandb anmelden

```Shell
wandb login
API-Key kopieren und einfügen, dann Enter drücken
```

![Das Bild zeigt den Vorgang der Anmeldung bei W&B (Weights & Biases) mit dem Befehl `wandb login` in der LeRobot-Umgebung. Nach Ausführung des Befehls weist W&B darauf hin, sich bei https://api.wandb.ai anzumelden, dass ein lokaler W&B-Server bereitgestellt werden kann, und leitet zur Suche nach dem API-Schlüssel an. Danach fordert W&B dazu auf, den API-Schlüssel einzufügen und Enter zu drücken, und weist darauf hin, dass keine .netrc-Datei gefunden wurde und gerade erstellt wird. Zuletzt zeigt W&B an, dass man als tommyzihao angemeldet ist, mit dem Anmeldestatus https://api.wandb.ai. Dieses Bild gehört zum Schritt „Bei wandb anmelden" im Dokument und veranschaulicht die Rückmeldung der Anmeldung.](../../en/images/d35-01.png)

## Den Datensatz einbinden

```Shell
Den Download-Befehl der Instanz kopieren, ähnlich wie:
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

Der Datensatz erscheint im Verzeichnis `~`

## Häufigkeit des Speicherns der Gewichte anpassen (optional)

Öffnen Sie `lerobot/src/lerobot/configs/train.py`

Ändern Sie save_freq von 20_000 auf 5_000

So erhalten Sie die Modellgewicht-Dateien bereits früher im Training
