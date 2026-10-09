[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | Português (BR) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# Visualizando e Reproduzindo um Conjunto de Dados

## Visualizar o Conjunto de Dados Inteiro

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

Digite `TommyZihao/lerobot_zihao_dataset_a`, ou outro conjunto de dados

![A imagem mostra a interface do LeRobot Dataset Visualizer, com um robô na imagem e as palavras "LeRobot Dataset Visualizer" no topo. No meio há um menu suspenso mostrando opções de conjuntos de dados como "TommyZihao/lerobot_zihao_dataset_a", juntamente com "Example Datasets" e nomes de conjuntos de dados abaixo, e um botão azul "Explore Open Datasets" na parte inferior. Esta imagem está relacionada à visualização do conjunto de dados inteiro mencionada acima, correspondendo à operação de digitar o conjunto de dados especificado.](../../en/images/d29-01.png)

![Imagem mostrando addCriterion addCriterion](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![A imagem mostra a interface de visualização do conjunto de dados grab-orange do LeRobot. No topo há um vídeo de alguém pegando uma laranja, com a laranja apoiada contra um objeto branco. Abaixo há gráficos de dados mostrando curvas de várias variáveis ao longo do tempo, como "actuator", "gripper" e "gripper_pos". À esquerda há uma lista de instruções, com "Grab Orangesanges" selecionada no momento. Os botões de reproduzir e pausar estão no canto inferior direito. Esta imagem está relacionada à visualização de um episódio específico, apresentando visualmente a ação de pegar e seus dados correspondentes.](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra a interface de visualização do conjunto de dados do LeRobot. À esquerda há uma linha do tempo que pode ser arrastada para ver a imagem em diferentes momentos, e no meio está o feed da câmera mostrando duas mãos em movimento.](../../en/images/d29-04.png)
</column>
</grid>

Observação: o comando e o estado não são a mesma coisa — o comando é fornecido pelo braço Leader, e o estado é fornecido pelo braço Follower

## Visualizar um Episódio Específico

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![A imagem mostra a interface para visualizar um episódio específico na plataforma rerun.io. À esquerda está a estrutura do conjunto de dados, mostrando dados como observation_images. No meio, no topo, está o feed ao vivo da câmera, com uma laranja na imagem. À direita estão curvas de dados mostrando como diferentes dados mudam ao longo do tempo. Na parte inferior há uma linha do tempo que pode ser arrastada para ver os dados em qualquer momento. Esta imagem corresponde a "Visualizar um Episódio Específico", apresentando visualmente a interface e os dados ao visualizar um episódio específico.](../../en/images/d29-05.png)

Arraste a linha do tempo para ver o feed da câmera e as posições dos servos em qualquer momento

## Reproduzir o Movimento do Braço Follower para um Episódio Específico

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

Você ouvirá `Replaying episode`, e então o braço Follower se move, reproduzindo e recriando o movimento do episódio especificado

Na verdade, a esta altura você já consegue impressionar muita gente leiga, não é mesmo

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
