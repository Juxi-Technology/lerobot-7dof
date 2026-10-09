[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | Deutsch | [Español](../es/01-install-lerobot/windows.md) | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Windows-Rechner

Der schwarze Leader-Arm verwendet ein 5V6A-Netzteil.

Der weiße Follower-Arm verwendet ein 12V5A-Netzteil.

## Miniconda installieren

anaconda.com/download/success

Oder klicken Sie auf diesen Link, um den Installer direkt herunterzuladen

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![Dieses Bild ist der Miniconda3-Installationsbildschirm unter Windows und zeigt die Softwareversion py312_24.7.1-0 (64-Bit). Der Bildschirm bietet zwei Optionen für den Installationstyp; die Option „Just Me (recommended)" ist rot umrandet und ist die aktuell ausgewählte, empfohlene Installationsart, während die andere Option „All Users (requires admin privileges)" nicht ausgewählt ist. Oben auf dem Bildschirm werden Sie aufgefordert, einen Installationstyp für Miniconda3 zu wählen, und unten befinden sich drei Schaltflächen: „Back", „Next" und „Cancel". Dieser Bildschirm ist der entscheidende Schritt im Miniconda-Installationsablauf, um den Umfang der Installation festzulegen.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![Das Bild zeigt die erweiterten Installationsoptionen auf dem Miniconda3-Installationsbildschirm. Die Option „Add Miniconda3 to my PATH environment variable" ist rot umrandet; daneben steht ein Hinweis, dass dies nicht empfohlen wird, da es zu Konflikten mit anderen Anwendungen kommen kann, und stattdessen die dem Windows-Startmenü hinzugefügten Menüs für die Eingabeaufforderung und PowerShell vorgeschlagen werden. Dieses Bild gehört zum Schritt des Erstellens einer virtuellen Umgebung nach „Den conda-Mirror ändern" und dient als Konfigurationsreferenz bei der Installation von Miniconda.](../../en/images/d05-02.png)
</column>
</grid>

## Den conda-Mirror ändern

```Shell
# Zuerst die vorhandene Mirror-Konfiguration löschen (um Konflikte zu vermeiden)
conda config --remove-key channels

# Die Standardkanäle von conda und gängige Drittanbieter-Kanäle durch den Tsinghua-Mirror ersetzen
# Die Standardpaketkanäle (main/r/msys2) hinzufügen
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# Gängige Drittanbieter-Kanäle hinzufügen
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# Die Anzeige der Downloadquelle einschalten, damit beim Installieren von Paketen die genaue Downloadadresse angezeigt wird
conda config --set show_channel_urls yes

# Den Index-Cache leeren, damit die neuen Mirrors wirksam werden
conda clean -i

# Die aktuelle Konfiguration anzeigen (um zu überprüfen, dass die Kanäle erfolgreich hinzugefügt wurden)
conda config --show-sources
```

## Eine virtuelle Umgebung erstellen

```Shell
conda create -y -n lerobot python=3.12
```

## Die virtuelle Umgebung aktivieren

```Shell
conda activate lerobot
```

## ffmpeg installieren

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

Überprüfen Sie, ob die Installation erfolgreich war

```Shell
ffmpeg
```

![Dieses Bild ist das Kommandozeilenfenster von Windows und zeigt das Überprüfungsergebnis nach Ausführung des Befehls ffmpeg. Konkret gibt die Kommandozeile ffmpeg Version 7.1.1 mit Copyright- und Build-Informationen aus, dazu Informationen zu den zugehörigen Bibliotheksdateien von ffmpeg sowie unten Hinweise zur Verwendung, die die grundlegende Nutzung und die Möglichkeit weiterer Hilfe abdecken. Dieses Bild dient dazu, zu überprüfen, dass ffmpeg auf einem Windows-Rechner erfolgreich installiert wurde; es entspricht dem Verifizierungsschritt nach „ffmpeg installieren" und veranschaulicht den Betriebszustand nach abgeschlossener ffmpeg-Installation.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![Dies ist die Terminaloberfläche von Linux und zeigt conda-bezogene Befehle und deren Ausführung. Zwei zentrale Befehle sind deutlich gekennzeichnet: der Befehl zum Aktivieren der virtuellen Umgebung mit dem Namen lerobot, „$ conda activate lerobot", und der Befehl zum Deaktivieren der aktiven Umgebung, „$ conda deactivate". Die Umgebung (base) ist derzeit aktiviert, und das Terminal führt die Installation von ffmpeg 7.1.1 aus dem Kanal conda-forge aus und zeigt mehrere konfigurierte Mirror-Adressen, während der Ablauf des Erfassens von Paket-Metadaten und der Abhängigkeitsumgebung bereits abgeschlossen ist.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![Das Bild zeigt die Terminaloberfläche bei der Verwendung des Befehls ffmpeg unter Ubuntu. Es zeigt die ffmpeg-Versionsinformationen, einschließlich Versionsnummer sowie Details zur Builder- und Compiler-Konfiguration. Außerdem werden die Versionen verschiedener Codecs aufgelistet, etwa libavcodec und libavformat. Unten stehen Verwendungshinweise, die Sie auffordern, „-h" für die vollständige Hilfe zu nutzen oder „man ffmpeg" auszuführen. Dieses Bild gehört zum Abschnitt „ffmpeg installieren" und dient dazu, eine erfolgreiche ffmpeg-Installation durch Anzeige ihrer Versions- und Build-Informationen zu überprüfen.](../../en/images/d05-05.png)
</column>
</grid>

## LeRobot herunterladen

- Das offizielle LeRobot-Repository herunterladen

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Das Code-Repository installieren

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![Das Bild zeigt das Überprüfungsergebnis in der Windows-Eingabeaufforderung (cmd) nach der Installation des LeRobot-Code-Repositories. Die Kommandozeile zeigt Meldungen wie „Successfully built lerobot", was auf eine erfolgreiche Installation hinweist. Außerdem werden mehrere Python-Pakete mit ihren Versionsnummern aufgelistet, etwa numpy 1.22.3 und scipy 1.7.1. Unten erscheint die Eingabeaufforderung „(lerobot) C:\\Users\\40743\\Downloads\\lerobot>", was anzeigt, dass das aktuelle Verzeichnis der Ordner lerobot unter Downloads ist. Dieses Bild entspricht dem Abschnitt „Die Installation überprüfen" und veranschaulicht die Rückmeldung der Kommandozeile nach einer erfolgreichen Installation.](../../en/images/d05-06.png)

## Die Installation überprüfen

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![Das Bild zeigt den Bildschirm zur Überprüfung einer erfolgreichen Installation in der Python-Umgebung unter Windows. Die Kommandozeile zeigt Python-Version 3.10.19, führte Code zum Importieren von Modulen wie lerobot, scservo_sdk und torch aus und führte schließlich torch.cuda.is_available() aus, was False zurückgab. Dieses Bild entspricht dem Abschnitt „Die Installation überprüfen" und veranschaulicht den Vorgang und das Ergebnis der Überprüfung einer erfolgreichen Installation über die Python-Umgebung nach der Installation des LeRobot-Code-Repositories.](../../en/images/d05-07.png)
