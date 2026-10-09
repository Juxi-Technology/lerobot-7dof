[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | Deutsch | [Español](../es/01-install-lerobot/mac.md) | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Mac-Rechner

Der schwarze Leader-Arm verwendet ein 5V6A-Netzteil.

Der weiße Follower-Arm verwendet ein 12V5A-Netzteil.

## Berechtigungen erteilen

![Das Bild zeigt ein Einstellungsfenster von macOS, aktuell auf der Seite „Bedienungshilfen", wobei in der linken Seitenleiste „Datenschutz & Sicherheit" ausgewählt ist. Das Fenster listet mehrere Anwendungen auf, darunter Baidu Netdisk, DingTalk und Doubao; der Schalter für die App Terminal ist rot umrandet und eingeschaltet. Dies entspricht dem Schritt „Berechtigungen erteilen" im Mac-Ablauf und aktiviert die nötigen Terminal-Berechtigungen als Vorbereitung für die Installation von Miniconda und das spätere Ändern der Mirrors.](../../en/images/d04-01.png)

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

![Das Bild zeigt die Terminalausgabe nach Ausführung des Befehls `ffmpeg`, mit der überprüft wird, ob ffmpeg erfolgreich installiert wurde; es entspricht dem Verifizierungsschritt nach „ffmpeg installieren". Die Ausgabe zeigt deutlich FFmpeg Version 7.1.1, das Copyright der FFmpeg-Entwickler von 2000 bis 2025, dazu Konfigurations-Flags, die Liste der unterstützten Encoder sowie Hinweise zur Verwendung des Universal Media Converter und schließt mit dem Tipp, für weitere Hilfe die Option `-h` oder den Befehl `man ffmpeg` zu verwenden.](../../en/images/d04-02.png)

## LeRobot herunterladen

- Das offizielle LeRobot-Repository herunterladen

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Das Code-Repository installieren

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![Das Bild zeigt, wie in der Kommandozeile auf einem Mac das Paket „feetech" mit pip installiert wird. Es stellt den Installationsfortschritt dar, darunter das Abrufen von Paketen von „https://repo.huaweicloud.com/repository/pypi/simple/" und das Herunterladen mehrerer Dateien wie datasets, diffusers und huggingface-hub, und endet mit dem Download von „einops==0.8.0". Dieses Bild gehört zum Abschnitt „Das Code-Repository installieren" und veranschaulicht die Befehlsausführung und das Ergebnis der Repository-Installation.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![Das Bild zeigt den Überprüfungsbildschirm auf einem Mac nach der Installation des LeRobot-Code-Repositories. Das Terminal zeigt „Successfully installed LeRobot" und listet mehrere installierte Python-Pakete mit ihren Versionen auf, etwa numpy und pandas. Dieses Bild entspricht den Abschnitten „Das Code-Repository installieren" und „Die Installation überprüfen" und zeigt die installierten Pakete, damit Anwender den Erfolg der Installation bestätigen können.](../../en/images/d04-04.png)
</column>
</grid>

## Die Installation überprüfen

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![Dieses Bild ist ein Screenshot der Mac-Terminal-Kommandozeile und gehört zur Überprüfung der Konfiguration in den LeRobot-Installationsschritten. Es zeigt das aktuelle Projektverzeichnis lerobot-main; nach dem Befehl python wird die interaktive Python-Umgebung mit Python-Version 3.10.19 unter dem System darwin betreten. Die Befehle import lerobot, import torch und torch.cuda.is_available() wurden nacheinander ausgeführt, wobei das Ergebnis für die CUDA-Verfügbarkeit False ergab, gefolgt von import scservo_sdk. Dies entspricht dem Verifizierungsschritt und dient dazu, den Installations- und Konfigurationsstatus von LeRobot und seinen Abhängigkeiten zu bestätigen.](../../en/images/d04-05.png)

![Das Bild zeigt die Terminalausgabe eines LeRobot-Befehls auf einem Mac. Es zeigt LeRobot Version 0.4.3, die Plattform macOS - 15.6.1 - arm64 - arm - 64bit, Python-Version 3.12.12 und weitere Informationen. Außerdem werden die Versionsinformationen von Huggingface Hub, Datasets, NumPy, FFmpeg und PyTorch aufgelistet, ob PyTorch mit CUDA-Unterstützung gebaut wurde, die CUDA-Version und das GPU-Modell sowie abschließend die Liste der LeRobot-Skripte. Dieses Bild gehört zum Kontext der Überprüfung und zeigt die LeRobot-Informationen nach der Installation.](../../en/images/d04-06.png)
