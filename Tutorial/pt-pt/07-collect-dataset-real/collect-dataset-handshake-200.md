[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | Português (PT)

# Recolher um conjunto de dados por demonstração — Handshake 200

## Criar um repositório de conjunto de dados no HuggingFace

https://huggingface.co/new-dataset

![A imagem mostra a interface para criar um novo repositório de conjunto de dados no HuggingFace. O "Owner" é apresentado como TommyZihao, o nome do conjunto de dados é "lerobot_zihao_dataset_shake200", a "License" está definida como mit e o tipo de conjunto de dados é "Public", visível por qualquer pessoa, embora apenas o proprietário do conjunto de dados ou os membros da organização possam fazer commits. Em baixo, indica que, após criar o conjunto de dados, pode carregar ficheiros através da interface web ou do git, e há um botão "Create dataset" na parte inferior. Esta imagem está relacionada com o conteúdo sobre criar um repositório de conjunto de dados no HuggingFace, e mostra a interface da operação de criar o conjunto de dados.](../../en/images/d27-01.png)

## Eliminar qualquer conjunto de dados existente com o mesmo nome (se existir)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Recolha do conjunto de dados Shake200

Uma câmara, a recolher um conjunto de dados - computador Mac

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
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake200 \
    --dataset.num_episodes=200 \
    --dataset.single_task="Shanke Hands" \
    --dataset.push_to_hub=false \
    --dataset.episode_time_s=12 \
    --dataset.reset_time_s=1
```

## Durante a recolha

<grid>
<column width-ratio="0.508765">
![Este é o ecrã do terminal durante a gravação do conjunto de dados de aperto de mão (Shake200). Tanto no topo como a meio-baixo da interface aparece a informação "INFO 202-04-15 17:00:22.227 Recording episode 36", mostrando que está a ser gravado o 36.º ciclo de recolha de dados (episode). Apresenta também os parâmetros relevantes do codificador SVT-AV1 e a informação de progresso "200/200 \[00:00:00::00, 4911.09 examples/s\]", refletindo que esta fase está a meio da recolha do conjunto de dados, correspondendo à secção "Durante a recolha" descrita no documento e mostrando o estado de funcionamento do sistema no momento da recolha.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![Esta imagem é a interface de execução do terminal do Mac, mostrando a configuração e os registos de execução do projeto SVT. A interface contém vários parâmetros de configuração, como o tamanho GOP, o tamanho mini-pod, o tipo de key-frame e os valores CRF da configuração do SVT, além da informação de versão da biblioteca SVT-A1 Encoder. Os registos da metade inferior registam operações relacionadas com ficheiros de vídeo, referindo a movimentação do atom para o início do ficheiro e os estados de ligação/desligação de vários componentes como OpenVCamera e SV280 follower, mostrando por fim a indicação de fim de execução do terminal no caminho atual — correspondendo ao cenário de execução do terminal durante a recolha por demonstração do conjunto de dados de aperto de mão 200.](../../en/images/d27-02.png)
</column>
</grid>

Controlos das setas do teclado:  
→ (seta para a direita) Termina o episódio atual antecipadamente; avança para o episódio seguinte.  
← (seta para a esquerda) Cancela o episódio atual; volta a gravá-lo.  
ESC, para imediatamente, codifica o vídeo e carrega o conjunto de dados.

## Recolha concluída — Diretório de gravação do conjunto de dados

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
