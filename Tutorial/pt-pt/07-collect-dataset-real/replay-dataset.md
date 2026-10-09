[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | Português (PT)

# Ver e reproduzir um conjunto de dados

## Visualizar todo o conjunto de dados

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

Introduza `TommyZihao/lerobot_zihao_dataset_a`, ou outro conjunto de dados

![A imagem mostra a interface do LeRobot Dataset Visualizer, com um robô na imagem e as palavras "LeRobot Dataset Visualizer" no topo. Ao centro há um menu pendente que mostra opções de conjuntos de dados como "TommyZihao/lerobot_zihao_dataset_a", juntamente com "Example Datasets" e nomes de conjuntos de dados por baixo, e um botão azul "Explore Open Datasets" em baixo. Esta imagem está relacionada com a visualização de todo o conjunto de dados mencionado acima, correspondendo à operação de introduzir o conjunto de dados especificado.](../../en/images/d29-01.png)

![Imagem a mostrar addCriterion addCriterion](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![A imagem mostra a interface de visualização do conjunto de dados de agarrar laranja do LeRobot. No topo está um vídeo de agarrar uma laranja, com a laranja segura contra um objeto branco. Em baixo estão gráficos de dados que mostram curvas de várias variáveis ao longo do tempo, como "actuator", "gripper" e "gripper_pos". À esquerda está uma lista de instruções, com "Grab Orangesanges" atualmente selecionado. Os botões de reproduzir e pausar estão no canto inferior direito. Esta imagem está relacionada com a visualização de um episódio específico, apresentando visualmente a ação de agarrar e os dados correspondentes.](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra a interface de visualização do conjunto de dados do LeRobot. À esquerda está uma linha temporal que pode ser arrastada para ver a imagem em diferentes momentos, e ao centro está a imagem da câmara que mostra duas mãos em movimento.](../../en/images/d29-04.png)
</column>
</grid>

Observação: o comando e o estado não são a mesma coisa — o comando é fornecido pelo braço Leader, e o estado é fornecido pelo braço Follower

## Visualizar um episódio específico

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![A imagem mostra a interface para visualizar um episódio específico na plataforma rerun.io. À esquerda está a estrutura do conjunto de dados, mostrando dados como observation_images. No meio, em cima, está a imagem da câmara ao vivo, com uma laranja na imagem. À direita estão curvas de dados que mostram como os diferentes dados variam ao longo do tempo. Na parte inferior está uma linha temporal que pode ser arrastada para ver os dados em qualquer momento. Esta imagem corresponde a "Visualizar um episódio específico", apresentando visualmente a interface e os dados ao visualizar um episódio específico.](../../en/images/d29-05.png)

Arraste a linha temporal para ver a imagem da câmara e as posições dos servos em qualquer momento

## Reproduzir o movimento do braço Follower para um episódio específico

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

Ouvirá `Replaying episode`, e depois o braço Follower move-se, reproduzindo e recriando o movimento do episódio especificado

Na verdade, a esta altura já consegue impressionar muita gente leiga, não é verdade

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
