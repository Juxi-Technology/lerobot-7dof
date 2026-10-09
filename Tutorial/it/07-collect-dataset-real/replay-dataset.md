[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | [Deutsch](../de/07-collect-dataset-real/replay-dataset.md) | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | Italiano | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# Visualizzare e riprodurre un dataset

## Visualizzare l'intero dataset

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

Inserisci `TommyZihao/lerobot_zihao_dataset_a`, o un altro dataset

![L'immagine mostra l'interfaccia di LeRobot Dataset Visualizer, con un robot raffigurato e la scritta "LeRobot Dataset Visualizer" in alto. Al centro c'è un menu a tendina che mostra opzioni di dataset come "TommyZihao/lerobot_zihao_dataset_a", insieme a "Example Datasets" e ai nomi dei dataset sottostanti, e sotto un pulsante blu "Explore Open Datasets". Questa immagine è collegata alla visualizzazione dell'intero dataset menzionata sopra e corrisponde all'operazione di inserimento del dataset specificato.](../../en/images/d29-01.png)

![Immagine che mostra addCriterion addCriterion](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![L'immagine mostra l'interfaccia di visualizzazione del dataset grab-orange di LeRobot. In alto c'è un video di presa di un'arancia, con l'arancia tenuta contro un oggetto bianco. Sotto ci sono grafici di dati che mostrano le curve di diverse variabili nel tempo, come "actuator", "gripper" e "gripper_pos". A sinistra c'è un elenco di istruzioni, con "Grab Orangesanges" attualmente selezionato. I pulsanti play e pausa sono in basso a destra. Questa immagine è collegata alla visualizzazione di un episodio specifico e presenta l'azione di presa e i dati corrispondenti.](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![L'immagine mostra l'interfaccia di visualizzazione del dataset di LeRobot. A sinistra c'è una timeline che può essere trascinata per vedere il fotogramma in momenti diversi, e al centro c'è il feed della telecamera che mostra due mani in movimento.](../../en/images/d29-04.png)
</column>
</grid>

Osservazione: il comando e lo stato non sono la stessa cosa — il comando è fornito dal braccio Leader, e lo stato è fornito dal braccio Follower

## Visualizzare un episodio specifico

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![L'immagine mostra l'interfaccia per visualizzare un episodio specifico sulla piattaforma rerun.io. A sinistra c'è la struttura del dataset, che mostra dati come observation_images. Al centro in alto c'è il feed live della telecamera, con un'arancia nell'immagine. A destra ci sono curve di dati che mostrano come i diversi dati cambiano nel tempo. In basso c'è una timeline che può essere trascinata per vedere i dati in qualsiasi momento. Questa immagine corrisponde a "Visualizzare un episodio specifico" e presenta l'interfaccia e i dati durante la visualizzazione di un episodio specifico.](../../en/images/d29-05.png)

Trascina la timeline per vedere il feed della telecamera e le posizioni dei servo in qualsiasi momento

## Riprodurre il movimento del braccio Follower per un episodio specifico

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

Sentirai `Replaying episode`, poi il braccio Follower si muove, riproducendo e ricreando il movimento dell'episodio specificato

In realtà, a questo punto puoi già impressionare molti profani, no?

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
