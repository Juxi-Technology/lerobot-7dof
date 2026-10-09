[English](../en/10-inference/infer-pi05.md) | [简体中文](../zh-hans/10-inference/infer-pi05.md) | [繁體中文](../zh-hant/10-inference/infer-pi05.md) | [Deutsch](../de/10-inference/infer-pi05.md) | [Español](../es/10-inference/infer-pi05.md) | [Français](../fr/10-inference/infer-pi05.md) | [Italiano](../it/10-inference/infer-pi05.md) | [日本語](../ja/10-inference/infer-pi05.md) | [한국어](../ko/10-inference/infer-pi05.md) | [Português (BR)](../pt-br/10-inference/infer-pi05.md) | Português (PT)

# Linha de Comandos de Inferência - pi0.5

## Ubuntu

- Eliminar o conjunto de dados existente com o prefixo eval (se existir)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- Linha de comandos de inferência

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi05/50K/pretrained_model
```















## Mac

- Eliminar o conjunto de dados existente com o prefixo eval (se existir)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Linha de comandos de inferência

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
![Esta imagem mostra a interface de linha de comandos para controlar um robô com Python 3.12 e o ambiente de simulação mujoco num ambiente Ubuntu. Mostra parte do código, a par de avisos e mensagens de erro que aparecem durante a execução: addCriterion](../../en/images/d52-01.png)
</column>
<column width-ratio="0.534183">
![Esta imagem mostra a saída da execução de código relacionado com Python 3.7.12 e PyTorch 1.12.0 num ambiente Ubuntu. Contém vários avisos e mensagens nos níveis "WARNING" e "INFO".](../../en/images/d52-02.png)
</column>
</grid>

## Porque É Que a Inferência É Lenta

- O conjunto de dados é demasiado pequeno
- A GPU não tem memória suficiente; precisa de uma placa da série 50
