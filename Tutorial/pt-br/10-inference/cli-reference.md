[English](../en/10-inference/cli-reference.md) | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | Português (BR) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# Referência da linha de comando

## Observações sobre a linha de comando

Com visualização ao vivo: --display_data=true

Sem visualização ao vivo: --display_data=false

Com `--display_data=true`, a interface bacana de visualização do rerun.io é iniciada, mas no diretório `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` é salva uma imagem para cada frame, o que ocupa muito espaço. Você pode defini-lo como `--display_data=false` depois.



Inferir um modelo a partir de um repositório de modelo do HuggingFace: --policy.path=Tommymy/lerobot_my_model_a



## Usando a tarefa Grab Oranges como exemplo

- Inferir um modelo local (com visualização ao vivo)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Inferir um modelo local (sem visualização ao vivo)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Inferir um modelo a partir de um repositório de modelo do HuggingFace

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --policy.path=Tommymy/lerobot_my_model_a
```

O modelo é baixado depois da execução

![Esta imagem mostra a interface para executar o script `pretrained_model.py` pela linha de comando. No topo ela mostra a configuração dos parâmetros do modelo, como `--display_data=true` e `--policy.path=TommyZihao/lerobot_zihao_model_a`. Abaixo há configurações de parâmetros como "robot", "camera" e "calibration_dir". Na parte inferior ela mostra o progresso de download do modelo, atualmente em 68%. A imagem está relacionada à descrição da execução do script `pretrained_model.py` e de seus parâmetros, apresentando visualmente as configurações dos parâmetros e o progresso do download.](../../en/images/d48-01.png)
