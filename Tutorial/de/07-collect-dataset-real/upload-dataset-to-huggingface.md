[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | Deutsch | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>Datensatz zu HuggingFace hochladen (optional)</title>

# Methode 1: Lokal hochladen (nicht empfohlen; langsame Upload-Geschwindigkeit)

- Automatischer Upload

Setzen Sie beim Aufnehmen des Datensatzes `push_to_hub=true`, dann wird er nach Abschluss der Aufnahme automatisch hochgeladen

![Dieses Bild zeigt eine Kommandozeilenoberfläche, einen Teil des Ausführungsprotokolls für den Datensatz-Uploadvorgang. Oben werden Umgebungsinformationen für Werkzeuge wie SVN und treet W2 angezeigt; in der Mitte steht eine Verarbeitungsmeldung wie „Starting the second pass: moving the mov atom to the beginning of the file", und darunter gibt es Fehler aus der Ausführung wie „error messaging the mach port for IMCRunLoopWakeUpReliable", während rechts der Verarbeitungsfortschritt und Datenübertragungswerte aufgelistet sind, etwa Datenmenge und Geschwindigkeit für verschiedene Einträge. Insgesamt stellt es eine Aufzeichnung des Betriebszustands während der Datensatz-Uploadverarbeitung dar.](../../en/images/d28-01.png)

- Manueller Upload

Setzen Sie beim Aufnehmen des Datensatzes `push_to_hub=false`, und laden Sie nach Abschluss der Aufnahme manuell hoch

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



Ob automatisch oder manuell: Die Upload-Geschwindigkeit ist sehr langsam (etwa 100 KB pro Sekunde)

weil die HuggingFace-Server im Ausland stehen

# Methode 2: Von einer Cloud-GPU-Plattform hochladen (empfohlen)

## Bei der Cloud-GPU-Plattform Featurize anmelden

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## Eine Cloud-GPU-Instanz starten

## Das Datensatz-Archiv zu `Datasets` hochladen

## Den Download-Befehl der Instanz kopieren

![Das Bild zeigt die Datensatzseite der Featurize-Plattform. Oben wird der Titel „Datasets" angezeigt, und darunter befindet sich ein Datensatz namens „soarm_amazing_hand_pick.zip", 213.3 MB groß, vor 16 Stunden hochgeladen. Rechts gibt es eine Schaltfläche „Cloud Unzip", zusammen mit Schaltflächen wie „Like", „Comment" und „Copy Instance Download Command". Dieses Bild gehört zum Schritt „Das Datensatz-Archiv zu `Datasets` hochladen" und zeigt die Datensatzseite nach dem Hochladen.](../../en/images/d28-02.png)

## In der Kommandozeile der Cloud-GPU-Instanz ausführen

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## Den Datensatz zu HuggingFace hochladen

Erstellen Sie eine Datei `upload_dataset.py` mit folgendem Inhalt

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

Führen Sie die Datei aus

```Shell
python upload_dataset.py
```

![Dieses Bild zeigt den Vorgang des Ausführens des Datensatz-Uploads in der Kommandozeile einer Cloud-GPU-Instanz, wobei ein Nutzer namens lerobot2 den Befehl python upload.py ausführte. Es zeigt den Verarbeitungsfortschritt der Dateien mit 6 zu verarbeitenden Dateien, die alle bei 100 % Fortschritt stehen, und markiert die Übertragungsgröße jeder Datei, wobei der Gesamtfortschritt der Datenübertragung bei 100 % liegt. Unten wird vermerkt, dass seit dem letzten Commit keine Dateien geändert wurden, sodass der Commit übersprungen wird, um keinen leeren Commit zu erzeugen. Dieser Inhalt entspricht dem Schritt des Ausführens der Datei upload_dataset.py.](../../en/images/d28-03.png)

- Eine weitere Upload-Methode (nicht empfohlen)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![Dieses Bild zeigt den Vorgang des Hochladens eines Datensatzes mit dem Befehl `hf upload` von HuggingFace, mit dem Befehl `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`. Das Bild zeigt, dass der Upload in seine Endphase eingetreten ist, wobei alle Dateien bei 100 % Fortschritt stehen, darunter mehrere Videodateien und Parquet-Dateien, deren Uploadgrößen genau mit den entsprechenden lokalen Dateigrößen übereinstimmen, zusammen mit der insgesamt hochgeladenen Dateigröße und der Übertragungsgeschwindigkeit, und unten ein Link zur HuggingFace-Datensatzseite für diesen Upload-Commit, was zeigt, dass die Uploadaufgabe abgeschlossen ist.](../../en/images/d28-04.png)

# Den Datensatz auf HuggingFace ansehen

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![Dieses Bild ist ein Screenshot der Detailseite des Datensatzes soarm_amazing_hand_pick des Juxi-Technology-Teams auf der Hugging Face-Plattform, entsprechend dem Inhalt „Den Datensatz auf HuggingFace ansehen". Oben werden Navigationsoptionen für den Datensatz und Basisinformationen wie Autor und Tags angezeigt; in der Mitte zeigt der Bereich Dataset Viewer einen Teil der Trainingsdaten für Split 1 des Datensatzes, einschließlich Feldern wie action, observation_state und timestamp und deren Werten, und markiert außerdem die Größe eines einzelnen Datensatzes, die Gesamtzahl der Datensätze und die Gesamtgröße. Unten werden außerdem zugehörige, mit diesen Daten trainierte Modelle erwähnt.](../../en/images/d28-05.png)

![Das Bild zeigt die Seite des Datensatzes soarm_amazing_hand_pick auf der Hugging Face-Plattform. Oben befinden sich ein Suchfeld und eine Navigationsleiste zum Suchen von Modellen, Datensätzen und so weiter. Der Abschnitt mit den Datensatzinformationen zeigt die besitzende Organisation Juxi - Technology und Tags wie robotics und imitation-learning. Unter dem Reiter „Files and versions" werden Ordner wie data, meta und videos sowie die Datei README.md aufgelistet, mit Angabe von Uploader, Uploadmethode und Zeitpunkt, etwa „Upload README.md with huggingface_hub". Dieses Bild gehört zum Ansehen eines HuggingFace-Datensatzes und veranschaulicht die Dateien und Versionen des Datensatzes.](../../en/images/d28-06.png)
