[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | [Deutsch](../de/10-inference/inference-dgx-spark.md) | Español | [Français](../fr/10-inference/inference-dgx-spark.md) | [Italiano](../it/10-inference/inference-dgx-spark.md) | [日本語](../ja/10-inference/inference-dgx-spark.md) | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# Inferencia en NVIDIA DGX Spark

## Instala el entorno

- PyTorch

Instala PyTorch aparte desde el sitio oficial, usando la versión CUDA 13.0

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![Esta imagen muestra los comandos y los resultados de instalar el entorno de LeRobot en el terminal. Primero ejecuta el comando "pip install -e /Downloads/lerobot", luego ejecuta "python -m lrobot -h" para ver la información de ayuda de LeRobot, que muestra la versión de LeRobot como 0.4.4. Por último ejecuta "pip show lrobot", que muestra el autor, la página principal y otra información de LeRobot. La imagen se relaciona con la instalación del entorno de LeRobot y presenta el proceso de instalación y sus resultados.](../../en/images/d56-01.png)

- Luego comenta torch por su cuenta en el archivo pyproject.toml

![Esta imagen muestra el contenido del archivo pyproject.toml, con la línea torchcode="2.3.0, c2.8.0" resaltada en un recuadro rojo. Este archivo es un archivo de configuración de proyecto de Python que sirve para especificar las dependencias del proyecto. El contexto menciona comentar torch por su cuenta en el archivo pyproject.toml y luego ejecutar pip install -e; la imagen se relaciona con ese contexto y presenta visualmente la ubicación de torchcode en el archivo pyproject.toml como referencia para el siguiente paso.](../../en/images/d56-02.png)

Luego ejecuta pip install -e .

- Nota: en la línea de comandos de inferencia, cambia la ruta policy.path a la ruta real del modelo dentro del Spark

## Elimina el conjunto de datos existente con prefijo eval (si lo hay)

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

- Instala el entorno

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- Inferencia

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

- Instala el entorno

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- Añade el código

![Esta imagen muestra parte del archivo factory.py en la carpeta policies del proyecto lerobot. El código clave dentro del recuadro rojo es "from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors", junto con instrucciones como "processors = make". La imagen se relaciona con la sección de inferencia del modelo pi0 y explica que este código debe añadirse para completar la operación de inferencia de pi0; es una parte importante del código de inferencia de pi0.](../../en/images/d56-03.png)

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

- Instala el entorno

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- Inferencia

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
