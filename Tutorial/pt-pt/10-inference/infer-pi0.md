[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | Português (PT)

# Linha de Comandos de Inferência - pi0

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![Esta imagem mostra um erro que surge ao ligar à máquina por SSH num ambiente Ubuntu. Mostra um erro de consola a indicar que a plataforma não é suportada, que não é possível estabelecer a ligação X e a aconselhar a garantir que está a correr um servidor X e que a variável de ambiente DISPLAY está definida corretamente. Mostra também um aviso sobre um ambiente headless e um registo do episódio 0 a ser gravado. A imagem relaciona-se com a linha de comandos de inferência em Ubuntu e pode ser uma situação anómala encontrada durante a operação.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![Esta imagem mostra a saída ao executar a linha de comandos de inferência num ambiente Ubuntu. Durante a execução, aparecem várias vezes mensagens de erro "E0119", indicando que não há uma configuração triton válida durante a autotuning e que os recursos estão esgotados, como memória partilhada insuficiente. Mostra também os parâmetros de execução de vários modelos triton_mm, como ALLOW_TF32, BLOCK_K e BLOCK_M, a par dos valores correspondentes de ACC_TYPE, ALLOW_TF32, BLOCK_K e BLOCK_M. A imagem relaciona-se com a linha de comandos de inferência em Ubuntu, mostrando uma escassez de recursos encontrada durante a execução.](../../en/images/d51-02.png)
</column>
</grid>

![Esta imagem mostra o terminal durante uma sessão de linha de comandos de inferência num ambiente Ubuntu. Mostra os resultados de várias instruções triton_mm, por exemplo triton_mm_3644 a demorar 0.2355 ms, todas a usar o tipo t1.float32 com ALLOW_TF32=True, e mostra também parâmetros como BLOCK_K. No fim mostra a avaliação comparativa SingleProcess AUTOTUNE a demorar 0.7305 segundos e 0.0001 segundos a pré-compilar 20 escolhas. A imagem relaciona-se com a linha de comandos de inferência em Ubuntu, mostrando a execução real.](../../en/images/d51-03.png)

> **Vídeo pendente**: o texto original incorpora aqui o `VID_20260120_182109.mp4` (originalmente 310 MB). Do lado do Feishu não foi disponibilizado nenhum fluxo de vídeo descarregável para este ficheiro, apenas metadados, pelo que não foi possível capturá-lo. Para o ver, consulte o [documento original](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg).



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
![Esta imagem mostra o terminal durante uma sessão de linha de comandos de inferência (11 - yolo26) num ambiente Ubuntu. Mostra informação da versão do Python 3.12 e um registo de robot-type definido como follower. Lista também parâmetros relacionados com a câmara, como color_mode, fourcc, fps, height e width, e mostra o caminho a partir do qual o modelo é carregado, a par de algumas mensagens de aviso, como erros de carregamento do modelo. A imagem relaciona-se com a linha de comandos de inferência em Ubuntu, apresentando o retorno do terminal durante a operação.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![Esta imagem mostra a saída da linha de comandos ao executar inferência com código Python num ambiente Ubuntu. Contém vários itens de informação, como o carregamento bem-sucedido do "PIBPytorch model", "WARNING" sobre chaves do modelo que podem precisar de ser tratadas, e "INFO" a indicar que a câmara OpenCV se ligou com sucesso. Mostra também várias vezes o aviso "huggingface/tokenizers: The process current just got forked...", sinalizando um problema de paralelismo causado pela bifurcação. A imagem relaciona-se com a linha de comandos de inferência em Ubuntu descrita no contexto, mostrando as várias mensagens e avisos que podem surgir em tempo de execução.](../../en/images/d51-05.png)
</column>
</grid>

## Porque É Que a Inferência num Mac Faz o Braço Tremer

- O conjunto de dados é demasiado pequeno
- A GPU não tem memória suficiente; precisa de uma placa da série 50
