[English](../en/07-collect-dataset-real/replay-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/replay-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/replay-dataset.md) | Deutsch | [Español](../es/07-collect-dataset-real/replay-dataset.md) | [Français](../fr/07-collect-dataset-real/replay-dataset.md) | [Italiano](../it/07-collect-dataset-real/replay-dataset.md) | [日本語](../ja/07-collect-dataset-real/replay-dataset.md) | [한국어](../ko/07-collect-dataset-real/replay-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/replay-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/replay-dataset.md)

# Einen Datensatz ansehen und wiedergeben

## Den gesamten Datensatz visualisieren

https://huggingface.co/spaces/lerobot/visualize_dataset

http://io-ai.tech/lerobot

https://open.platform.io-ai.tech

Geben Sie `TommyZihao/lerobot_zihao_dataset_a` oder einen anderen Datensatz ein

![Das Bild zeigt die Oberfläche des LeRobot Dataset Visualizer mit einem Roboter im Bild und dem Schriftzug „LeRobot Dataset Visualizer" oben. In der Mitte befindet sich ein Dropdown-Menü mit Datensatzoptionen wie „TommyZihao/lerobot_zihao_dataset_a", zusammen mit „Example Datasets" und Datensatznamen darunter, sowie unten eine blaue Schaltfläche „Explore Open Datasets". Dieses Bild gehört zum oben erwähnten Visualisieren des gesamten Datensatzes und entspricht dem Vorgang der Eingabe des angegebenen Datensatzes.](../../en/images/d29-01.png)

![Bild, das addCriterion addCriterion zeigt](../../en/images/d29-02.png)



<grid>
<column width-ratio="0.500000">
![Das Bild zeigt die Visualisierungsoberfläche des LeRobot-Datensatzes zum Greifen einer Orange. Oben ist ein Video vom Greifen einer Orange, wobei die Orange vor einem weißen Objekt gehalten wird. Darunter sind Datendiagramme, die Kurven mehrerer Variablen über die Zeit zeigen, etwa „actuator", „gripper" und „gripper_pos". Links ist eine Liste von Anweisungen, wobei derzeit „Grab Orangesanges" ausgewählt ist. Wiedergabe- und Pausenschaltflächen befinden sich unten rechts. Dieses Bild gehört zur Visualisierung einer bestimmten Episode und veranschaulicht die Greifbewegung und die zugehörigen Daten.](../../en/images/d29-03.png)
</column>
<column width-ratio="0.500000">
![Das Bild zeigt die Visualisierungsoberfläche des LeRobot-Datensatzes. Links ist eine Zeitleiste, die sich ziehen lässt, um das Bild zu verschiedenen Zeitpunkten anzusehen, und in der Mitte ist das Kamerabild, das zwei Hände in Bewegung zeigt.](../../en/images/d29-04.png)
</column>
</grid>

Hinweis: Der Befehl und der Zustand sind nicht dasselbe — der Befehl kommt vom Leader-Arm, und der Zustand kommt vom Follower-Arm

## Eine bestimmte Episode visualisieren

```Shell
lerobot-dataset-viz --repo-id TommyZihao/lerobot_zihao_dataset_a --episode-index=2
```

![Das Bild zeigt die Oberfläche zur Visualisierung einer bestimmten Episode auf der rerun.io-Plattform. Links ist die Datensatzstruktur, die Daten wie observation_images zeigt. In der Mitte oben ist das Live-Kamerabild mit einer Orange im Bild. Rechts sind Datenkurven, die zeigen, wie sich verschiedene Daten über die Zeit verändern. Unten ist eine Zeitleiste, die sich ziehen lässt, um die Daten zu jedem Zeitpunkt anzusehen. Dieses Bild entspricht „Eine bestimmte Episode visualisieren" und veranschaulicht die Oberfläche und die Daten beim Ansehen einer bestimmten Episode.](../../en/images/d29-05.png)

Ziehen Sie die Zeitleiste, um Kamerabild und Servopositionen zu jedem Zeitpunkt anzusehen

## Die Bewegung des Follower-Arms für eine bestimmte Episode wiedergeben

```Shell
lerobot-replay \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
    --dataset.episode=2
```

Sie hören `Replaying episode`, dann bewegt sich der Follower-Arm und gibt die Bewegung der angegebenen Episode wieder

Eigentlich können Sie damit schon jede Menge Laien beeindrucken, nicht wahr

<figure view-type="Preview">[Attachment: VID_20260115_140050.mp4](../../en/images/VID_20260115_140050.mp4)</figure>
