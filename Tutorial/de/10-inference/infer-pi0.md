[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | Deutsch | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# Inferenz-Kommandozeile – pi0

## Ubuntu

- Löschen Sie den vorhandenen, mit eval-präfixten Datensatz (falls vorhanden)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- Inferenz-Kommandozeile

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![Dieses Bild zeigt einen Fehler, der beim Verbinden mit dem Rechner über SSH in einer Ubuntu-Umgebung auftritt. Es zeigt einen Konsolenfehler mit der Meldung, dass die Plattform nicht unterstützt werde, dass die X-Verbindung nicht hergestellt werden könne, und dem Hinweis, sicherzustellen, dass ein X-Server läuft und die Umgebungsvariable DISPLAY korrekt gesetzt ist. Außerdem zeigt es eine Warnung zu einer Headless-Umgebung und einen Eintrag, dass Episode 0 aufgezeichnet wird. Das Bild bezieht sich auf die Ubuntu-Inferenz-Kommandozeile und kann eine anormale Situation sein, die im Betrieb auftritt.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![Dieses Bild zeigt die Ausgabe beim Ausführen der Inferenz-Kommandozeile in einer Ubuntu-Umgebung. Während der Ausführung erscheinen mehrfach Fehlermeldungen „E0119", wonach beim Autotuning keine gültige Triton-Konfiguration vorliegt und Ressourcen erschöpft sind, etwa unzureichender Shared Memory. Außerdem zeigt es die Laufzeitparameter mehrerer triton_mm-Modelle wie ALLOW_TF32, BLOCK_K und BLOCK_M, zusammen mit den entsprechenden Werten für ACC_TYPE, ALLOW_TF32, BLOCK_K und BLOCK_M. Das Bild bezieht sich auf die Ubuntu-Inferenz-Kommandozeile und zeigt eine während des Laufs aufgetretene Ressourcenknappheit.](../../en/images/d51-02.png)
</column>
</grid>

![Dieses Bild zeigt das Terminal während einer Inferenz-Kommandozeilensitzung in einer Ubuntu-Umgebung. Es zeigt die Ergebnisse mehrerer triton_mm-Anweisungen, zum Beispiel triton_mm_3644 mit 0,2355 ms, alle unter Verwendung des Typs t1.float32 mit ALLOW_TF32=True, und zeigt außerdem Parameter wie BLOCK_K. Am Ende zeigt es das SingleProcess-AUTOTUNE-Benchmarking mit 0,7305 Sekunden und 0,0001 Sekunden zum Vorkompilieren von 20 Auswahlmöglichkeiten. Das Bild bezieht sich auf die Ubuntu-Inferenz-Kommandozeile und zeigt die tatsächliche Ausführung.](../../en/images/d51-03.png)

> **Video ausstehend**: Der Originaltext bettet hier `VID_20260120_182109.mp4` ein (ursprünglich 310 MB). Auf der Feishu-Seite wurde für diese Datei kein herunterladbarer Videostream bereitgestellt, nur Metadaten, sodass sie nicht erfasst werden konnte. Zum Ansehen siehe das [Originaldokument](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg).



## Mac

- Löschen Sie den vorhandenen, mit eval-präfixten Datensatz (falls vorhanden)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inferenz-Kommandozeile

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![Dieses Bild zeigt das Terminal während einer Inferenz-Kommandozeilensitzung (11 - yolo26) in einer Ubuntu-Umgebung. Es zeigt Versionsinformationen zu Python 3.12 und einen Eintrag, dass der Robotertyp auf follower gesetzt wurde. Außerdem listet es kamera­bezogene Parameter wie color_mode, fourcc, fps, height und width auf und zeigt den Pfad, aus dem das Modell geladen wird, zusammen mit einigen Warnmeldungen wie Modellladefehler. Das Bild bezieht sich auf die Ubuntu-Inferenz-Kommandozeile und zeigt die Terminalrückmeldungen während des Betriebs.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![Dieses Bild zeigt die Kommandozeilenausgabe beim Ausführen der Inferenz mit Python-Code in einer Ubuntu-Umgebung. Es enthält mehrere Informationen, etwa das erfolgreiche Laden „PIBPytorch model", „WARNING" zu Modellschlüsseln, die möglicherweise behandelt werden müssen, und „INFO", dass die OpenCV-Kamera erfolgreich verbunden wurde. Außerdem zeigt es mehrfach die Warnung „huggingface/tokenizers: The process current just got forked...", die auf ein durch den Fork verursachtes Parallelitätsproblem hinweist. Das Bild bezieht sich auf die im Kontext beschriebene Ubuntu-Inferenz-Kommandozeile und zeigt die verschiedenen Meldungen und Warnungen, die zur Laufzeit erscheinen können.](../../en/images/d51-05.png)
</column>
</grid>

## Warum die Inferenz auf einem Mac den Arm stottern lässt

- Der Datensatz ist zu klein
- Die GPU hat nicht genug Speicher; Sie benötigen eine Karte der 50er-Serie
