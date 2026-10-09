[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | Deutsch | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# Inferenz auf D-Robotics RDK S100

Den detaillierten Implementierungsablauf finden Sie unter diesem Link<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy Full Workflow Document" type="doc"></cite>



## End-to-End-Bereitstellung des ACT-Modells auf RDK S100/S100P

Dieser Abschnitt führt Sie durch den vollständigen Bereitstellungszyklus für das ACT-Modell auf der Hardware der Serie D-Robotics RDK S100. Der gesamte Prozess umfasst drei Kernphasen: **Modellexport**, **Quantisierungs-Kompilierung** und **Ausführung auf dem Board**.

<callout emoji="💡">
**Voraussetzungen:**
- **Entwicklungsrechner (Host):** wird verwendet, um die Schritte 1 und 2 auszuführen, üblicherweise Ihr Modell-Trainingsrechner (er benötigt ordentliche Leistung und eine installierte Docker-Umgebung).
- **Board (Edge):** das D-Robotics RDK S100/S100P, wird verwendet, um Schritt 3 auszuführen.
- **Toolchain:** Dieser Artikel stützt sich auf das Repository `rdk_LeRobot_tools`; Details finden Sie im [GitHub-Repository](https://github.com/D-Robotics/rdk_LeRobot_tools).
</callout>

<callout emoji="🚨">
**Wichtiger Hinweis zur Versionskompatibilität (unbedingt lesen):** Der aktuelle ONNX-Exportablauf von `rdk_LeRobot_tools` ist vollständig kompatibel mit **LeRobot-Datensätzen v2.1**. Da die neueste Version v3.0 die Datenstruktur ändert, wird **dringend empfohlen**, das Haupt-`lerobot`-Repository vor den Arbeiten in diesem Abschnitt auf den spezifischen Commit umzuschalten, der mit v2.1 kompatibel ist, damit der Exportablauf reibungslos läuft. 
*Empfohlene Commit-ID:* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### Phase 1: Modell in das ONNX-Format exportieren 💻 (auf dem Entwicklungsrechner)

Zunächst müssen wir das **mit PyTorch trainierte** Modell in ein Zwischenformat (ONNX) exportieren.



#### **1. Das Toolchain-Repository klonen** 

Gehen Sie in Ihr Arbeitsverzeichnis `lerobot` und klonen Sie die RDK-spezifische Toolchain:

```Bash
cd lerobot

# 1. Auf die stabile, mit v2.1-Datensätzen kompatible Version umschalten
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. Die D-Robotics-RDK-spezifische Toolchain klonen
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. Die Exportparameter konfigurieren** 

Bearbeiten Sie die Datei `rdk_LeRobot_tools/bpu_export_config.yaml` und passen Sie die Konfiguration an Ihre tatsächlichen Pfade an:

```YAML
dataset:
  root: "data/so101_pick_place" # absoluter oder relativer Pfad zu Ihrem Datensatz
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # Pfad zu den ursprünglichen PyTorch-Modellgewichten
type: "nash-e" # Zielhardware-Architektur; RDK S100 entspricht nash-e / S100P entspricht nash-m
```



#### 3. Das Exportskript ausführen

```Bash
# ONNX exportieren (Entwicklungsrechner)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **Erfolgsanzeige**: Im aktuellen Verzeichnis wird ein Ordner `bpu_export_output` angelegt, der das Skript `build_all.sh` und die später benötigten Quantisierungskalibrierungsdaten enthält.



### Phase 2: Das BPU-Modell kompilieren 🐳 (in einer Docker-Umgebung auf dem Entwicklungsrechner)

Das Quantisieren und Kompilieren von D-Robotics-BPU-Modellen erfordert eine OpenExplorer-(OE-)Umgebung. Wir empfehlen, die Umgebung mit Docker zu isolieren.



#### **1.** **Docker-Umgebung und Image vorbereiten** 

Stellen Sie sicher, dass Docker auf dem Entwicklungsrechner installiert ist ([offizielle Installationsanleitung](https://docs.docker.com/engine/install/)). Laden Sie das empfohlene CPU-Image herunter und laden Sie es:

```Bash
# Das heruntergeladene Offline-Image-Archiv laden
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. Den Kompilierungscontainer starten**

<callout emoji="⚠️">
**Warnung vor einer Stolperfalle**: Das Kompilieren des Modells benötigt eine große Menge Shared Memory. Fügen Sie unbedingt das Argument `--shm-size=15g` hinzu, sonst sind IPC-Speicherfehler sehr wahrscheinlich.
</callout>

Binden Sie das Arbeitsverzeichnis des Entwicklungsrechners (mit dem gerade exportierten Ordner) in den Container ein:

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(Hinweis: Ersetzen Sie `<docker-image-name>` durch den tatsächlichen Image-Namen, den Sie über `sudo docker images` sehen.)



#### **3.** **Die Kompilierung im Container ausführen** 

Sobald Sie sich im Container befinden, führen Sie das Ein-Klick-Kompilierungsskript aus:

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **Die Build-Artefakte prüfen** 

Nach Abschluss der Kompilierung wird unter `bpu_export_output` ein Ordner `bpu_output/` angelegt. Er enthält alle Kern-Dateien, die zum Ausführen auf dem RDK-Board benötigt werden: 

- Klicken Sie, um die Verzeichnisstruktur von `bpu_output/` anzuzeigen

  - `BPU_ACTPolicy_TransformerLayers.hbm` (quantisierte Modelldatei)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (quantisierte Modelldatei)
  - `action_mean.npy` und mehrere weitere Normalisierungsparameter des Datensatzes
  - `camera1_mean.npy` und weitere Kamera-Statistikparameter

---

### Phase 3: Bereitstellung auf dem Board und Inferenz 🤖 (auf dem RDK S100)

<callout emoji="📌">
**Prüfung der Voraussetzungen:**
1. Auf dem RDK-Board ist bereits die Laufzeitumgebung `D-Robotics/lerobot` eingerichtet, mit installiertem `hbm_runtime`.
2. Der gesamte im vorherigen Schritt erzeugte Ordner `bpu_output/` wurde vollständig auf das RDK-Board kopiert, per `scp`, USB-Stick oder Ähnlichem.
3. Die grundlegende Teleoperations-Konfiguration ist bereits erledigt, sodass der serielle Port des Arms, der USB-Port der Kamera und die Kalibrierungsdatei korrekt konfiguriert sind.
</callout>



#### **1.** **BPU-beschleunigte Inferenz ausführen**

Gehen Sie im Terminal auf dem RDK-Board in das Toolchain-Verzeichnis und starten Sie das Steuerungsskript:

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ Fehlerbehebung

Wenn bei einer tatsächlichen Bereitstellung Probleme auftreten, prüfen Sie anhand der folgenden Liste:

- **Der Arm bewegt sich nicht?**

  - Prüfen Sie, ob das Gerät eingebunden ist: Geben Sie im Terminal `ls /dev/ttyACM*` ein und bestätigen Sie, dass der serielle Port des Arms korrekt ist.
  - Prüfen Sie die Berechtigungen: Versuchen Sie, das Inferenzskript mit `sudo` auszuführen, oder fügen Sie den aktuellen Benutzer zur Gruppe `dialout` hinzu.
- **Kamera-Streaming-Fehler / anormales Bild / der Arm zittert auf der Stelle?**

  - Bestätigen Sie, ob der Kamera-Index wegen eines Hot-Plugs verrutscht ist, und prüfen Sie, ob die Kameraparameter im Code zu den tatsächlichen `/dev/video*` passen.
- **Beim Kopieren von vom Container erzeugten Dateien auf dem Entwicklungsrechner erscheint „unzureichende Berechtigungen"?**

  - In einem Docker-eingebundenen Verzeichnis erzeugte Dateien gehören standardmäßig root; führen Sie auf dem Entwicklungsrechner `sudo chown -R $USER:$USER bpu_export_output` aus, um das zu beheben.
