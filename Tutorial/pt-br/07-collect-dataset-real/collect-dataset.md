[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | Português (BR) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# Coletando um Conjunto de Dados por Demonstração

## Excluir Qualquer Conjunto de Dados Existente com o Mesmo Nome (Se Houver)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## Uma Câmera, Coletando um Conjunto de Dados - computador Mac

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

## Duas Câmeras, Coletando um Conjunto de Dados - computador Mac

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

## Durante a Coleta

<grid>
<column width-ratio="0.508765">
![A imagem mostra a interface do terminal durante a coleta de um conjunto de dados com OpenVSLAM em um Mac. No topo, ela mostra os parâmetros de coleta, como resolução, taxa de quadros e codificador. Abaixo está o log de coleta, registrando o horário de início da coleta, informações de versão, número de threads e codificador, e também mostrando o progresso da coleta, como 298/298 episódios coletados, 5119.33 segundos no total. Na parte inferior há observações para a tecla "ESC", como parar imediatamente e enviar o conjunto de dados. Esta imagem está relacionada ao fluxo de trabalho de coleta do conjunto de dados, apresentando visualmente o retorno do terminal durante a coleta.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![Esta imagem mostra a interface de linha de comando do terminal em um Mac, usada para exibir informações de log de execução relacionadas à coleta de conjunto de dados por câmera. Ela contém parâmetros de configuração relacionados ao SVT, como parâmetros de configuração, a versão da biblioteca de codificação e os valores de cada item de configuração (como quadro-chave e CRF, resolução de codificação), e também mostra logs de status de execução, como mensagens sobre processamento de arquivos MP4, registros de desconexão de dispositivo e carimbos de data e hora durante a execução do programa. No geral, apresenta o estado de execução em segundo plano durante a coleta de conjunto de dados por câmera.](../../en/images/d26-02.png)
</column>
</grid>

Controles pelas teclas de seta do teclado:  
→ (Seta para a direita) Encerra o episódio atual antecipadamente; passa para o próximo episódio.  
← (Seta para a esquerda) Cancela o episódio atual; grava-o novamente.  
ESC, para imediatamente, codifica o vídeo e envia o conjunto de dados.

## Coleta Concluída — Diretório de Salvamento do Conjunto de Dados

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
