[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | Português (PT)

# Recolher um conjunto de dados por demonstração

## Eliminar qualquer conjunto de dados existente com o mesmo nome (se existir)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## Uma câmara, a recolher um conjunto de dados - computador Mac

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## Duas câmaras, a recolher um conjunto de dados - computador Mac

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.num_episodes=40 \
    --dataset.single_task="Grab Oranges" \
    --dataset.push_to_hub=true \
    --dataset.episode_time_s=10 \
    --dataset.reset_time_s=2
```

## Durante a recolha

<grid>
<column width-ratio="0.508765">
![A imagem mostra a interface do terminal durante a recolha de um conjunto de dados com o OpenVSLAM num Mac. No topo mostra os parâmetros de recolha, como resolução, taxa de fotogramas e codificador. Em baixo está o registo da recolha, registando a hora de início da recolha, informação de versão, número de threads e codificador, e mostrando também o progresso da recolha, como 298/298 episódios recolhidos, 5119.33 segundos no total. Na parte inferior há notas para a tecla "ESC", como parar imediatamente e carregar o conjunto de dados. Esta imagem está relacionada com o fluxo de trabalho de recolha de conjuntos de dados, apresentando visualmente o feedback do terminal durante a recolha.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![Esta imagem mostra a interface do terminal da linha de comandos num Mac, utilizada para apresentar informação de registo de execução relacionada com a recolha de conjuntos de dados por câmara. Contém parâmetros de configuração relacionados com o SVT, como parâmetros de configuração, a versão da biblioteca de codificação e os valores de cada item de configuração (como key frame e CRF, resolução de codificação), e mostra também registos do estado em tempo de execução, como mensagens sobre o processamento de ficheiros MP4, registos de desconexão de dispositivos e marcas temporais durante a execução do programa. De um modo geral, apresenta o estado de execução em segundo plano durante a recolha de conjuntos de dados por câmara.](../../en/images/d26-02.png)
</column>
</grid>

Controlos das setas do teclado:  
→ (seta para a direita) Termina o episódio atual antecipadamente; avança para o episódio seguinte.  
← (seta para a esquerda) Cancela o episódio atual; volta a gravá-lo.  
ESC, para imediatamente, codifica o vídeo e carrega o conjunto de dados.

## Recolha concluída — Diretório de gravação do conjunto de dados

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```





## Handshake

```Shell
lerobot-record \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.num_episodes=30 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```
