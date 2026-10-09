[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | [Deutsch](../de/10-inference/inference-dgx-spark.md) | [Español](../es/10-inference/inference-dgx-spark.md) | [Français](../fr/10-inference/inference-dgx-spark.md) | Italiano | [日本語](../ja/10-inference/inference-dgx-spark.md) | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# Inferenza su NVIDIA DGX Spark

## Installare l'ambiente

- PyTorch

Installa PyTorch separatamente dal sito ufficiale, usando la versione con CUDA 13.0

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![Questa immagine mostra i comandi e i risultati dell'installazione dell'ambiente LeRobot nel terminale. Esegue dapprima il comando "pip install -e /Downloads/lerobot", poi esegue "python -m lrobot -h" per visualizzare le informazioni di aiuto di LeRobot, che mostrano la versione di LeRobot pari a 0.4.4. Infine esegue "pip show lrobot", che visualizza l'autore, la homepage e altre informazioni di LeRobot. L'immagine è collegata all'installazione dell'ambiente LeRobot e presenta il processo di installazione e i suoi risultati.](../../en/images/d56-01.png)

- Poi commenta torch da solo nel file pyproject.toml

![Questa immagine mostra il contenuto del file pyproject.toml, con la riga torchcode="2.3.0, c2.8.0" evidenziata in un riquadro rosso. Questo file è un file di configurazione di un progetto Python usato per specificare le dipendenze del progetto. Il contesto menziona il fatto di commentare torch da solo nel file pyproject.toml e poi eseguire pip install -e; l'immagine è collegata a quel contesto e presenta visivamente la posizione di torchcode nel file pyproject.toml come riferimento per il passaggio successivo.](../../en/images/d56-02.png)

Poi esegui pip install -e .

- Nota: nella riga di comando di inferenza, cambia il percorso policy.path con il percorso effettivo del modello all'interno dello Spark

## Elimina il dataset esistente con prefisso eval (se presente)

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

- Installare l'ambiente

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- Inferenza

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

- Installare l'ambiente

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- Aggiungi il codice

![Questa immagine mostra parte del file factory.py nella cartella policies del progetto lerobot. Il codice chiave nel riquadro rosso è "from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors", insieme a istruzioni come "processors = make". L'immagine è collegata alla sezione di inferenza del modello pi0 e spiega che questo codice deve essere aggiunto per completare l'operazione di inferenza di pi0; è una parte importante del codice di inferenza di pi0.](../../en/images/d56-03.png)

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

- Installare l'ambiente

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- Inferenza

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
