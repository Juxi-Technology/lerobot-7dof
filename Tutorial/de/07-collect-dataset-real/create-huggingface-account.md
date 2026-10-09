[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | Deutsch | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Ein Hugging-Face-Konto registrieren (optional)

## Einen in China gehosteten HuggingFace-Mirror einrichten

- Ubuntu

```Shell
sudo nano ~/.bashrc

# Dies am Ende der Datei hinzufügen
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# Ausgabe
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# Dies am Ende der Datei hinzufügen
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# Ausgabe
# https://hf-mirror.com
```



## Einen Token erstellen

https://huggingface.co/settings/tokens

![Das Bild zeigt die Seite „Settings / Tokens" der Hugging Face-Website. Links befinden sich der Avatar und die Profilinformationen des Nutzers, rechts der Bereich „User Access Tokens", der erklärt, dass Zugriffstoken zur Authentifizierung dienen und Anwendungen erlauben, basierend auf den Token-Berechtigungen Aktionen auszuführen, und betont, den Token nicht mit anderen zu teilen. Oben rechts befindet sich die Schaltfläche „+ Create new token" zum Erstellen eines neuen Tokens. Dieses Bild gehört zum Abschnitt „Einen Token erstellen" des Dokuments und zeigt den Einstiegspunkt zum Erstellen eines Tokens.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![Dieses Bild zeigt die Seite zum Erstellen eines neuen Zugriffsnachweises (Token) auf der Hugging Face-Plattform. Im Bereich „Token type" ist die Option „Write" hervorgehoben, das Eingabefeld „Token name" ist bereits mit „lerobot_zihao_token" ausgefüllt, unten auf der Seite gibt es die Schaltfläche „Create token" zum Absenden des Erstellvorgangs, und links auf der Seite werden die Kontoinformationen des aktuell angemeldeten Nutzers angezeigt.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![Das Bild zeigt das Popup-Fenster „Save your Access Token" auf der Hugging Face-Website. Das Popup erinnert den Nutzer daran, den Token-Wert an einem sicheren Ort aufzubewahren, da er nach dem Schließen nicht mehr angezeigt werden kann. Darunter wird der Token-Wert „<your-huggingface-token>" mit einer Schaltfläche „Copy" zum Kopieren des Tokens angezeigt. Außerdem sind der Token-Name „lerobot_zihao_token" und die Berechtigung „WRITE" angegeben. Dieses Bild gehört zum Schritt „Einen Token erstellen" im Dokument und dient dazu, Nutzer beim Notieren des erzeugten Tokens anzuleiten.](../../en/images/d24-03.png)
</column>
</grid>

## Den Token notieren

Beispielsweise meiner lautet:

```Shell
<your-huggingface-token>
```

## Den Token binden

```Shell
hf auth login

hf auth whoami
```

![Das Bild zeigt die Bedienung eines Hugging Face-Kontos über die Kommandozeile unter macOS. Zunächst wird der Befehl „hf auth login" ausgeführt, der meldet, dass bereits ein Token vorhanden ist, und dass man mit „hf auth whoami" Informationen abrufen oder sich mit „hf auth logout" abmelden kann. Anschließend wird der Token eingegeben; nach der Bestätigung wird angezeigt, dass der Token im angegebenen Pfad gespeichert wurde und der aktuell aktive Token „lerobot zihao token" ist. Zuletzt wird der Befehl „hf auth whoami" ausgeführt, der den Nutzer als „TommyZihao" anzeigt. Dieses Bild entspricht dem Abschnitt „Einen Token erstellen" im Dokument und zeigt den Vorgang des Token-Erstellens und dessen Ergebnis.](../../en/images/d24-04.png)

## Ein Dataset-Repo erstellen

<grid>
<column width-ratio="0.480604">
![Dies ist ein Screenshot der Hugging Face-Plattform. Oben links befinden sich Einstiege wie Community, Docs und Pricing; in der Mitte der Oberfläche stehen die Informationen des persönlichen Kontos TommyZihao, und ein grüner Rahmen markiert das Nutzer-Avatar-Symbol oben rechts. Im mittleren Menü ist die Option „+ New Dataset" mit einem roten Rahmen hervorgehoben; diese Option entspricht dem Schritt „Ein Dataset-Repo erstellen" und startet beim Anklicken die Erstellung eines Datensatz-Repositories.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![Das Bild zeigt die Oberfläche zum Erstellen eines neuen Datensatz-Repositories auf Hugging Face. Unter „Dataset name" ist „lerobot_zihao_dataset_a" eingegeben, „License" ist auf „mit" gesetzt, und die Option „Public" ist angehakt. Unten befindet sich die Schaltfläche „Create dataset". Dieses Bild entspricht dem Abschnitt „Ein Dataset-Repo erstellen" im Dokument und zeigt die wichtigsten auszufüllenden Informationen beim Erstellen eines Datensatz-Repositories sowie die Oberfläche und die erforderlichen Felder.](../../en/images/d24-06.png)
</column>
</grid>

![Dies ist die Weboberfläche der Hugging Face-Plattform. Sie zeigt den Inhalt des vom Nutzer TommyZhao erstellten Dataset-Repositories namens lexobot_zihao_dataset_a im Dark-Theme-Modus. Links auf der Seite gibt es die Bearbeitungsoption „Edit Dataset Card", außerdem werden mehrere Möglichkeiten zum Hochladen von Datensatzdateien mit Beispielbefehlen für CLI, Python, HTTPS und SSH angeboten, und die License des Datensatzes ist als mit angegeben.](../../en/images/d24-07.png)
