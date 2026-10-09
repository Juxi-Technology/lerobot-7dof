[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | [Deutsch](../de/10-inference/inference-dgx-spark.md) | [Español](../es/10-inference/inference-dgx-spark.md) | Français | [Italiano](../it/10-inference/inference-dgx-spark.md) | [日本語](../ja/10-inference/inference-dgx-spark.md) | [한국어](../ko/10-inference/inference-dgx-spark.md) | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# Inférence sur NVIDIA DGX Spark

## Installer l'environnement

- PyTorch

Installez PyTorch séparément depuis le site officiel, en utilisant la version CUDA 13.0

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![Cette image montre les commandes et les résultats de l'installation de l'environnement LeRobot dans le terminal. Elle exécute d'abord la commande « pip install -e /Downloads/lerobot », puis « python -m lrobot -h » pour consulter l'aide de LeRobot, qui affiche la version de LeRobot comme 0.4.4. Enfin, elle exécute « pip show lrobot », qui affiche l'auteur, la page d'accueil et d'autres informations de LeRobot. L'image se rapporte à l'installation de l'environnement LeRobot, présentant le processus d'installation et ses résultats.](../../en/images/d56-01.png)

- Ensuite, commentez torch de son côté dans le fichier pyproject.toml

![Cette image montre le contenu du fichier pyproject.toml, avec la ligne torchcode="2.3.0, c2.8.0" mise en évidence dans un cadre rouge. Ce fichier est un fichier de configuration de projet Python servant à spécifier les dépendances du projet. Le contexte mentionne le fait de commenter torch de son côté dans le fichier pyproject.toml puis d'exécuter pip install -e ; l'image se rapporte à ce contexte, présentant visuellement l'emplacement de torchcode dans le fichier pyproject.toml comme référence pour l'étape suivante.](../../en/images/d56-02.png)

Exécutez ensuite pip install -e .

- Remarque : dans la ligne de commande d'inférence, remplacez le chemin policy.path par le chemin réel du modèle à l'intérieur du Spark

## Supprimer le jeu de données existant préfixé par eval (le cas échéant)

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

- Installer l'environnement

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- Inférence

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

- Installer l'environnement

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- Ajouter le code

![Cette image montre une partie du fichier factory.py dans le dossier policies du projet lerobot. Le code clé dans le cadre rouge est « from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors », accompagné d'instructions telles que « processors = make ». L'image se rapporte à la section d'inférence du modèle pi0, expliquant que ce code doit être ajouté pour mener à bien l'opération d'inférence pi0 ; il fait partie intégrante du code d'inférence pi0.](../../en/images/d56-03.png)

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

- Installer l'environnement

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- Inférence

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
