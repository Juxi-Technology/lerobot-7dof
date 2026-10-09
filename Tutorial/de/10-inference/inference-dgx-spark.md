[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | Deutsch | [Español](../es/10-inference/inference-dgx-spark.md) | [Français](../fr/10-inference/inference-dgx-spark.md) | [Italiano](../it/10-inference/inference-dgx-spark.md) | [日本語](../ja/10-inference/inference-dgx-spark.md) | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# Inferenz auf NVIDIA DGX Spark

## Umgebung installieren

- PyTorch

Installieren Sie PyTorch separat von der offiziellen Seite, in der CUDA-13.0-Version

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![Dieses Bild zeigt die Befehle und Ergebnisse beim Installieren der LeRobot-Umgebung im Terminal. Zuerst wird der Befehl „pip install -e /Downloads/lerobot" ausgeführt, dann „python -m lrobot -h", um die LeRobot-Hilfeinformationen anzuzeigen, wobei die LeRobot-Version als 0.4.4 erscheint. Zuletzt wird „pip show lrobot" ausgeführt, was Autor, Homepage und weitere Informationen zu LeRobot anzeigt. Das Bild bezieht sich auf die Installation der LeRobot-Umgebung und zeigt den Installationsvorgang und seine Ergebnisse.](../../en/images/d56-01.png)

- Kommentieren Sie dann torch in der Datei pyproject.toml separat aus

![Dieses Bild zeigt den Inhalt der Datei pyproject.toml, wobei die Zeile torchcode="2.3.0, c2.8.0" in einem roten Rahmen hervorgehoben ist. Diese Datei ist eine Konfigurationsdatei für ein Python-Projekt und dient zur Angabe der Projektabhängigkeiten. Der Kontext erwähnt, torch in der Datei pyproject.toml separat auszukommentieren und dann pip install -e auszuführen; das Bild bezieht sich auf diesen Kontext und zeigt die Position von torchcode in der Datei pyproject.toml als Referenz für den nächsten Schritt.](../../en/images/d56-02.png)

Führen Sie dann pip install -e . aus

- Hinweis: Ändern Sie in der Inferenz-Kommandozeile den Pfad policy.path auf den tatsächlichen Modellpfad im Spark

## Vorhandenen mit eval-präfixten Datensatz löschen (falls vorhanden)

```Shell
sudo chmod 666 /dev/ttyACM*
```



```Shell
sudo rm -rf /home/apx103/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

## ACT

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
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/ACT/5K/pretrained_model
```

## SmolVLA

- Umgebung installieren

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- Inferenz

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
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## WALL-OSS

- Umgebung installieren

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- Den Code ergänzen

![Dieses Bild zeigt einen Teil der Datei factory.py im Ordner policies des lerobot-Projekts. Der zentrale Code im roten Rahmen lautet „from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors", zusammen mit Anweisungen wie „processors = make". Das Bild bezieht sich auf den Abschnitt zur Inferenz des pi0-Modells und erklärt, dass dieser Code ergänzt werden muss, um die pi0-Inferenz abzuschließen; er ist ein wichtiger Bestandteil des pi0-Inferenzcodes.](../../en/images/d56-03.png)

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

## pi0

- Umgebung installieren

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- Inferenz

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
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```
