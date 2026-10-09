[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | [Deutsch](../de/10-inference/inference-dgx-spark.md) | [Español](../es/10-inference/inference-dgx-spark.md) | [Français](../fr/10-inference/inference-dgx-spark.md) | [Italiano](../it/10-inference/inference-dgx-spark.md) | [日本語](../ja/10-inference/inference-dgx-spark.md) | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | Português (PT)

# Inferência NVIDIA DGX Spark

## Instalar o Ambiente

- PyTorch

Instale o PyTorch separadamente a partir do sítio oficial, usando a versão CUDA 13.0

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![Esta imagem mostra os comandos e resultados da instalação do ambiente LeRobot no terminal. Primeiro executa o comando "pip install -e /Downloads/lerobot", depois executa "python -m lrobot -h" para ver a informação de ajuda do LeRobot, que mostra a versão do LeRobot como 0.4.4. Por fim executa "pip show lrobot", que apresenta o autor, a página inicial e outra informação do LeRobot. A imagem relaciona-se com a instalação do ambiente LeRobot, apresentando o processo de instalação e os seus resultados.](../../en/images/d56-01.png)

- Em seguida, comente o torch isoladamente no ficheiro pyproject.toml

![Esta imagem mostra o conteúdo do ficheiro pyproject.toml, com a linha torchcode="2.3.0, c2.8.0" realçada numa caixa vermelha. Este ficheiro é um ficheiro de configuração de projeto Python usado para especificar dependências do projeto. O contexto menciona comentar o torch isoladamente no ficheiro pyproject.toml e depois executar pip install -e; a imagem relaciona-se com esse contexto, apresentando visualmente a localização do torchcode no ficheiro pyproject.toml como referência para o passo seguinte.](../../en/images/d56-02.png)

Depois execute pip install -e .

- Nota: na linha de comandos de inferência, altere o caminho policy.path para o caminho real do modelo dentro do Spark

## Eliminar o conjunto de dados existente com o prefixo eval (se existir)

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

- Instalar o Ambiente

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- Inferência

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

- Instalar o Ambiente

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- Adicionar o código

![Esta imagem mostra parte do ficheiro factory.py na pasta policies do projeto lerobot. O código essencial na caixa vermelha é "from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors", a par de instruções como "processors = make". A imagem relaciona-se com a secção de inferência do modelo pi0, explicando que este código tem de ser adicionado para completar a operação de inferência do pi0; é uma parte importante do código de inferência do pi0.](../../en/images/d56-03.png)

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

- Instalar o Ambiente

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- Inferência

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
