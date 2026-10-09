[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | Deutsch | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# Ein Modell zu HuggingFace hochladen (optional)

## Ein Modell-Repo erstellen

<grid>
<column width-ratio="0.354197">
![Dieses Bild zeigt die HuggingFace-Nutzeroberfläche. Es zeigt ein Avatarsymbol; ein Klick darauf öffnet ein Drop-down-Menü, in dem die Option „New Model" mit einem roten Rahmen hervorgehoben ist. Das Bild bezieht sich auf den Abschnitt „Ein Modell zu HuggingFace hochladen (optional)" und entspricht dem Schritt „Ein Modell-Repo erstellen"; es veranschaulicht den Einstiegspunkt zum Anlegen eines neuen Modells auf HuggingFace und hilft dem Nutzer zu verstehen, wie modellbezogene Ressourcen auf der Plattform erstellt werden.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![Dieses Bild zeigt die Oberfläche zum Anlegen eines neuen Modell-Repositorys auf der HuggingFace-Website. Das Drop-down „Owner" ist auf „TommyZihao" gesetzt, das Feld „Model name" enthält „lerobot_zihao_model_a" und das Feld „License" enthält „mit". Darunter befinden sich eine Option „Base template" und die Repository-Typauswahl „Public" und „Private". Das Bild bezieht sich auf den Abschnitt „Ein Modell-Repo erstellen" und ist ein Beispiel für das Ausfüllen der Angaben beim Anlegen eines Modell-Repositorys.](../../en/images/d46-02.png)
</column>
</grid>

## Das Modell-Repo ansehen

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

Es ist vorerst leer

![Dieses Bild zeigt die Modellseite „TommyZihao/lerobot_zihao_model_a" auf der HuggingFace-Plattform. Links befindet sich ein Reiter „Model card" zum Bearbeiten der Modellkarte. Rechts erklärt der Abschnitt „Getting started with your model", wie man das Modell verwendet, einschließlich des Hinzufügens vollständiger Modellinformationen und des Pushs der Modelldateien. Darunter können Sie im Bereich „Edit Model Card" Lizenz, Sprache, Basis-Modell und weitere Informationen des Modells ergänzen. Unten bietet der Bereich „Push your model files" mehrere Möglichkeiten zum Hochladen von Modelldateien, darunter CLI, Python, Git, HTTPS und SSH. Das Bild bezieht sich auf das Hochladen eines Modells zu HuggingFace und zeigt die Bedienelemente der Seite.](../../en/images/d46-03.png)

![Dieses Bild zeigt die Seite des Modell-Repos TommyZihao/lerobot_zihao_model_a auf der HuggingFace-Plattform. Die Seite zeigt die Dateigröße des Modells mit 1,54 KB, einen Mitwirkenden und einen Verlauf von 1 Commit, der vor 9 Minuten erstellt wurde. Außerdem listet sie die Dateien .gitattributes und README.md mit 1,52 KB bzw. 24 Bytes Größe auf, ebenfalls aus dem ersten Commit, ebenfalls vor 9 Minuten. Das Bild bezieht sich auf das Hochladen eines Modells zu HuggingFace und zeigt, wie die Seite nach dem Hochladen des Modells aussieht.](../../en/images/d46-04.png)

## Das Modell hochladen

Erstellen Sie eine Datei `upload_model.py` mit folgendem Inhalt

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

Führen Sie sie aus

```Shell
python upload_model.py
```

![Dieses Bild zeigt die Ausgabe beim Ausführen des Befehls `python upload_model.py` von der Kommandozeile. Es zeigt den Dateiverarbeitungsfortschritt bei 34 % und den Upload-Fortschritt der neuen Daten ebenfalls bei 34 %, und listet den Upload-Fortschritt für die beiden Dateien `d_model/model.safetensors` und `tokenizer_processor.safetensors` mit jeweils 92 %. Das Bild bezieht sich auf das Hochladen eines Modells zu HuggingFace und zeigt den Fortschritt beim Hochladen von Modelldateien.](../../en/images/d46-05.png)

## Das Modell-Repo ansehen

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![Dieses Bild zeigt die HuggingFace-Repo-Seite für TommyZihaos Modell lerobot_zihao_model_a. Die Seite zeigt die Lizenz des Modells als „mit", einen Mitwirkenden und einen Verlauf von 2 Commits. In der Mitte listet sie mehrere Dateien auf, etwa README.md, config.json und model.safetensors, jeweils mit dem Text „Upload folder using huggingface_hub" rechts daneben, was zeigt, dass diese Dateien über huggingface_hub hochgeladen wurden. Das Bild bezieht sich auf das Hochladen eines Modells zu HuggingFace und zeigt, wie die Modelldateien auf HuggingFace gespeichert werden.](../../en/images/d46-06.png)

Jetzt sind die Modelldateien vorhanden
