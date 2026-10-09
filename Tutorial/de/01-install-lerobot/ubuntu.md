[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | Deutsch | [Español](../es/01-install-lerobot/ubuntu.md) | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Ubuntu-Rechner

Der schwarze Leader-Arm verwendet ein 5V6A-Netzteil.

Der weiße Follower-Arm verwendet ein 12V5A-Netzteil.

## Miniconda installieren

https://www.anaconda.com/download

## Den pip-Mirror ändern

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## Den conda-Mirror ändern

```Shell
# Die vorhandene .condarc-Konfiguration leeren (optional, um Konflikte zu vermeiden)
echo "" > ~/.condarc

# Die Konfiguration des Tsinghua-Mirrors schreiben
cat << EOF > ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
EOF

# Den Cache leeren, damit die Konfiguration wirksam wird
conda clean -i
```

## Eine virtuelle Umgebung erstellen

```Shell
conda create -y -n lerobot python=3.12 -y
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

<grid>
<column width-ratio="0.568354">
![Das Bild zeigt das Ergebnis des Aktivierens einer conda-virtuellen-Umgebung und der Installation von ffmpeg auf einem Ubuntu-Rechner. Zuerst aktiviert conda die virtuelle Umgebung mit dem Namen lerobot, dann wird der Befehl conda install ffmpeg=7.1.1 -c conda -forge ausgeführt, der die Channels-Informationen von conda einschließlich conda - forge anzeigt, und schließlich die Platform linux - 64 zusammen mit den abgeschlossenen Operationen Collecting package metadata und Solving environment. Dieses Bild entspricht dem Abschnitt „ffmpeg installieren" und veranschaulicht die Ausführung der Installation.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![Dieses Bild ist ein Screenshot des Terminals eines Ubuntu-Systems und zeigt das nach Ausführung des Befehls ffmpeg zurückgegebene Ergebnis. Es zeigt ffmpeg Version 7.1.1, dessen Konfigurationsinformationen und die Versionsnummern der unterstützten Module (etwa libavcodec und libavformat), zusammen mit Hinweisen zur Verwendung des universal media converter. Es entspricht dem Verifizierungsschritt nach der Installation von ffmpeg und dient dazu, zu bestätigen, dass das Werkzeug ffmpeg auf dem System erfolgreich installiert wurde.](../../en/images/d03-02.png)
</column>
</grid>

## Das offizielle LeRobot-Repository herunterladen

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Das Code-Repository installieren

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![Dieses Bild zeigt, wie im Terminal eines Ubuntu-Systems in das Verzeichnis lerobot gewechselt und der Befehl `pip install -e.\[feetech\]` ausgeführt wird, was Teil der Installation des offiziellen LeRobot-Repositories ist. Es zeigt deutlich jeden Schritt der Befehlsausführung, darunter das Abrufen von Paketen aus dem angegebenen Huawei-Cloud-Repository, das Installieren von Abhängigkeiten und das Herunterladen zugehöriger Datensatz-Pakete (etwa diffusers, huggingface-hub und accelerate), wobei bei mehreren Paketen der jeweilige Download-Fortschritt, die Größe und die Geschwindigkeit angegeben sind, und endet mit der Meldung, dass die Abhängigkeiten bereits erfüllt sind, womit die Repository-Installation abgeschlossen ist.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![Dieses Bild zeigt die Kommandozeilenoberfläche des Terminals eines Ubuntu-Systems während der Installation von Softwarepaketen, einschließlich Informationen zur Abhängigkeitsbehandlung bei der Installation von Software wie ffmpeg. Das Terminal zeigt die Liste der verarbeiteten Pakete, etwa pytz, pyyaml und numpy, sowie den Vorgang des Deinstallierens vorhandener Paketversionen und der Installation neuer Versionen, mit Hinweisen zur Abhängigkeitskonsistenz. Dieser Inhalt entspricht dem Schritt „Die Installation überprüfen" nach „ffmpeg installieren" und ist eine Aufzeichnung der Terminalausgabe aus der Überprüfung des Installationsvorgangs von ffmpeg und anderer Software.](../../en/images/d03-03.png)
</column>
</grid>

## Die Installation überprüfen

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## Ergebnisse auf einem 4090-Host

![Das Bild zeigt die auf einem Ubuntu-Rechner laufenden LeRobot-Befehle und -Informationen. Der Befehl „Lerobot lerobot -info" zeigt LeRobot Version 0.4.3, die Plattform Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, Python-Version 3.12.0 und weitere Informationen. Darin ist die PyTorch-Version 2.7.1 + cu126, die CUDA-Version 12.6 und das GPU-Modell NVIDIA GeForce RTX 4090. Dieses Bild gehört zur Überprüfung einer erfolgreichen Installation und zeigt die Betriebsinformationen von LeRobot in der Ubuntu-Umgebung.](../../en/images/d03-04.png)

![Das Bild zeigt die interaktive Python-Oberfläche beim Ausführen des LeRobot-Repositories auf einem Ubuntu-Rechner. Es zeigt Python-Version 3.10.12, einschließlich der conda-forge-Verpackungsinformationen und der Kompilierungszeit. Der Nutzer gab nacheinander die Befehle `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()` und `import scservo_sdk` ein und erhielt die LeRobot-Versionsnummer 0.4.3, die CUDA-Verfügbarkeit True und einen erfolgreichen Import von `scservo_sdk`. Dieses Bild gehört zur Überprüfung der Installation des LeRobot-Repositories und veranschaulicht den Verifizierungsvorgang.](../../en/images/d03-05.png)

## Ergebnisse auf einem NVIDIA DGX Spark

![Das Bild zeigt die Terminalausgabe beim Ausführen des LeRobot-Repositories auf einem Ubuntu-Rechner. Es zeigt LeRobot Version 0.4.4, die CUDA-Version 13.0 und das GPU-Modell NVIDIA GeForce GTX 1660 Ti. Außerdem werden Bibliotheksversionen wie HuggingFace Hub, Datasets und PyTorch sowie die Werkzeugversionen von FFmpeg und PyTorch aufgelistet. Abschließend werden die Importe von LeRobot und torch überprüft; torch.cuda.is_available() gibt True zurück, was bedeutet, dass CUDA verfügbar ist. Dieses Bild gehört zum Ausführen des LeRobot-Repositories auf einem Ubuntu-Rechner und zeigt die Ergebnisse.](../../en/images/d03-06.png)
