[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | [Deutsch](../de/10-inference/infer-smolvla.md) | [Español](../es/10-inference/infer-smolvla.md) | [Français](../fr/10-inference/infer-smolvla.md) | [Italiano](../it/10-inference/infer-smolvla.md) | [日本語](../ja/10-inference/infer-smolvla.md) | [한국어](../ko/10-inference/infer-smolvla.md) | Português (BR) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# Linha de comando de inferência - smolvla

## Ubuntu

- Exclua o conjunto de dados existente com prefixo eval (se houver)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Linha de comando de inferência

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## Mac

- Exclua o conjunto de dados existente com prefixo eval (se houver)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Linha de comando de inferência

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/smolvla/40K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=2000
```

<grid>
<column width-ratio="0.425772">
![Esta imagem mostra a interface da linha de comando de inferência em um ambiente Ubuntu. No topo ela mostra o comando sendo executado, incluindo configurações de parâmetros como o uso do cache e o uso de Delta Joint Actions Aloha. Abaixo há várias informações principais, como o id do robô "zihao_follower_arm", um alvo relativo máximo de None, a porta "/dev/tty.usbmodemSAAF2193661" e uma observação de que o número de camadas do VLM foi reduzido para 16. A imagem está relacionada à linha de comando de inferência no Ubuntu, apresentando a interface e algumas das principais configurações de parâmetros.](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![Esta imagem mostra o terminal durante uma sessão de linha de comando de inferência em um ambiente Ubuntu. No topo ela mostra configurações relacionadas ao robô, como calibration_dir e cameras. Abaixo há barras de progresso de carregamento de vários arquivos json, como config.json e processor_config.json, mostrando a porcentagem e o tamanho de carregamento. Na parte inferior há mensagens de log como "Mismatch between calibration values in the motor and the calibration file or no calibration file found", sinalizando uma incompatibilidade entre os valores de calibração do motor e o arquivo de calibração. A imagem corresponde à linha de comando de inferência no Ubuntu, mostrando o retorno do terminal durante a operação.](../../en/images/d50-02.png)
</column>
</grid>

## Resultados

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
