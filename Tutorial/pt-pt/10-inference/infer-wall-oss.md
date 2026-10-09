[English](../en/10-inference/infer-wall-oss.md) | [简体中文](../zh-hans/10-inference/infer-wall-oss.md) | [繁體中文](../zh-hant/10-inference/infer-wall-oss.md) | [Deutsch](../de/10-inference/infer-wall-oss.md) | [Español](../es/10-inference/infer-wall-oss.md) | [Français](../fr/10-inference/infer-wall-oss.md) | [Italiano](../it/10-inference/infer-wall-oss.md) | [日本語](../ja/10-inference/infer-wall-oss.md) | [한국어](../ko/10-inference/infer-wall-oss.md) | [Português (BR)](../pt-br/10-inference/infer-wall-oss.md) | Português (PT)

# Linha de Comandos de Inferência - WALL-OSS

## Ubuntu

- Eliminar o conjunto de dados existente com o prefixo eval (se existir)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Linha de comandos de inferência

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

![Esta imagem mostra o terminal durante uma sessão de linha de comandos de inferência num ambiente Ubuntu. Mostra várias definições de parâmetros, como uma contagem total de pixels de vídeo de 90316800 e um tamanho de ficheiro de configuração de pré-processamento de 2.46 KB. Mostra também o progresso de carregamento de ficheiros como tokenizer.json e tokenizer_config.json, por exemplo tokenizer.json a carregar a 100%. No topo há mensagens "INFO" e, por baixo, pedidos de carregamento do modelo como "Loading model from:". A imagem corresponde à linha de comandos de inferência em Ubuntu, apresentando a informação essencial durante a operação.](../../en/images/d53-01.png)

<figure view-type="Preview">[Attachment: c7b8a795e52ca10689d296d212dc8e53.mp4](../../en/images/c7b8a795e52ca10689d296d212dc8e53.mp4)</figure>





## Linha de Comandos de Inferência - Mac

- Eliminar o conjunto de dados existente com o prefixo eval (se existir)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Linha de comandos de inferência

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

<figure view-type="Preview">[Attachment: 9f4567abdcce7f983415aef8f42877d0.mp4](../../en/images/9f4567abdcce7f983415aef8f42877d0.mp4)</figure>
