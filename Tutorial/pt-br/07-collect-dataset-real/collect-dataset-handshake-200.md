[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | Português (BR) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# Coletando um Conjunto de Dados por Demonstração — Handshake 200

## Criar um Repositório de Dataset no HuggingFace

https://huggingface.co/new-dataset

![A imagem mostra a interface de criação de um novo repositório de conjunto de dados no HuggingFace. Em "Owner" aparece TommyZihao, o campo "Dataset name" está preenchido com "lerobot_zihao_dataset_shake200", "License" está definida como mit e a opção "Public" está selecionada, com uma observação abaixo de que qualquer pessoa pode visualizar este conjunto de dados e somente o proprietário do conjunto de dados ou membros da organização podem fazer commits. Na parte inferior há a observação de que, após criar o conjunto de dados, você pode enviar arquivos pela interface web ou por git, além de um botão "Create dataset". Esta imagem está relacionada ao conteúdo sobre criar um Repositório de Dataset no HuggingFace e mostra a interface da operação de criação do conjunto de dados.](../../en/images/d27-01.png)

## Excluir Qualquer Conjunto de Dados Existente com o Mesmo Nome (Se Houver)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Coleta do Conjunto de Dados Shake200

Uma câmera, coletando um conjunto de dados - computador Mac

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

## Durante a Coleta

<grid>
<column width-ratio="0.508765">
![Esta é a imagem da tela do terminal durante a gravação para coletar o conjunto de dados de aperto de mão (Shake200). Tanto no topo quanto na parte central inferior da interface há a informação marcada "INFO 202-04-15 17:00:22.227 Recording episode 36", indicando que o 36º episódio de coleta de dados está sendo gravado. Também são apresentados os parâmetros relevantes do codificador SVT-AV1 e a informação de progresso "200/200 \[00:00:00::00, 4911.09 examples/s\]", mostrando que esta etapa está em meio à coleta do conjunto de dados, correspondendo à etapa "Durante a Coleta" descrita no documento e exibindo o estado de execução do sistema no momento da coleta.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![Esta imagem é a interface de execução do terminal do Mac, mostrando a configuração e os logs de execução do projeto SVT. A interface contém vários parâmetros de configuração, como o tamanho de GOP, o tamanho de mini-pod, o tipo de quadro-chave e os valores de CRF da configuração do SVT, além de informações de versão da biblioteca SVT-A1 Encoder. Os logs da metade inferior registram operações relacionadas a arquivos de vídeo, mencionando mover o átomo para o início do arquivo, além dos estados de conexão e desconexão de vários componentes como OpenVCamera e SV280 follower, e por fim exibe o aviso de conclusão da execução do terminal no caminho atual, correspondendo ao cenário de execução do terminal durante a coleta de demonstração do conjunto de dados de aperto de mão 200.](../../en/images/d27-02.png)
</column>
</grid>

Operações com as teclas de seta do teclado:  
→ (Seta para a direita) encerra o episódio atual antecipadamente; passa para o próximo episódio.  
← (Seta para a esquerda) cancela o episódio atual; grava novamente.  
ESC, para imediatamente, codifica o vídeo e envia o conjunto de dados.

## Coleta Concluída, Diretório de Salvamento do Conjunto de Dados

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
