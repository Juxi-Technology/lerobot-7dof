[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | Português (BR) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# Linha de comando de inferência - pi0

## Ubuntu

- Exclua o conjunto de dados existente com prefixo eval (se houver)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
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
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![Esta imagem mostra um erro que aparece ao se conectar à máquina via SSH em um ambiente Ubuntu. Ela mostra um erro de console informando que a plataforma não é compatível, que a conexão X não pode ser estabelecida e aconselhando a garantir que um servidor X esteja em execução e que a variável de ambiente DISPLAY esteja definida corretamente. Também mostra um aviso sobre um ambiente headless e um registro do episódio 0 sendo gravado. A imagem está relacionada à linha de comando de inferência no Ubuntu e pode ser uma situação anormal encontrada durante a operação.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![Esta imagem mostra a saída ao executar a linha de comando de inferência em um ambiente Ubuntu. Durante a execução, mensagens de erro "E0119" aparecem várias vezes, informando que não há uma configuração triton válida durante o autotuning e que os recursos se esgotaram, como memória compartilhada insuficiente. Ela também mostra os parâmetros de tempo de execução de vários modelos triton_mm, como ALLOW_TF32, BLOCK_K e BLOCK_M, junto com os valores correspondentes de ACC_TYPE, ALLOW_TF32, BLOCK_K e BLOCK_M. A imagem está relacionada à linha de comando de inferência no Ubuntu, mostrando uma escassez de recursos encontrada durante a execução.](../../en/images/d51-02.png)
</column>
</grid>

![Esta imagem mostra o terminal durante uma sessão de linha de comando de inferência em um ambiente Ubuntu. Ela mostra os resultados de várias instruções triton_mm, por exemplo triton_mm_3644 levando 0.2355 ms, todas usando o tipo t1.float32 com ALLOW_TF32=True, e também mostra parâmetros como BLOCK_K. No final ela mostra o benchmarking SingleProcess AUTOTUNE levando 0.7305 segundos e 0.0001 segundos para pré-compilar 20 escolhas. A imagem está relacionada à linha de comando de inferência no Ubuntu, mostrando a execução real.](../../en/images/d51-03.png)

> **Vídeo pendente**: o texto original incorpora `VID_20260120_182109.mp4` (originalmente 310 MB) aqui. Do lado do Feishu, nenhum stream de vídeo para download foi fornecido para este arquivo, apenas metadados, então ele não pôde ser capturado. Para vê-lo, consulte o [documento original](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg).



## Mac

- Exclua o conjunto de dados existente com prefixo eval (se houver)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Linha de comando de inferência

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
![Esta imagem mostra o terminal durante uma sessão de linha de comando de inferência (11 - yolo26) em um ambiente Ubuntu. Ela mostra informações da versão do Python 3.12 e um registro de robot-type definido como follower. Também lista parâmetros relacionados à câmera, como color_mode, fourcc, fps, height e width, e mostra o caminho a partir do qual o modelo é carregado, junto com algumas mensagens de aviso, como erros de carregamento do modelo. A imagem está relacionada à linha de comando de inferência no Ubuntu, apresentando o retorno do terminal durante a operação.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![Esta imagem mostra a saída da linha de comando ao executar a inferência com código Python em um ambiente Ubuntu. Ela contém vários itens de informação, como "PIBPytorch model" carregado com sucesso, "WARNING" sobre chaves de modelo que podem precisar ser tratadas e "INFO" indicando que a câmera OpenCV se conectou com sucesso. Ela também mostra o aviso "huggingface/tokenizers: The process current just got forked..." várias vezes, sinalizando um problema de paralelismo causado pelo fork. A imagem está relacionada à linha de comando de inferência no Ubuntu descrita no contexto, mostrando as várias mensagens e avisos que podem aparecer em tempo de execução.](../../en/images/d51-05.png)
</column>
</grid>

## Por que a inferência em um Mac faz o braço tremer

- O conjunto de dados é pequeno demais
- A GPU não tem memória suficiente; você precisa de uma placa da série 50
