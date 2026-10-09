[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | Deutsch | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# Einen Datensatz durch Vorführen aufnehmen

## Einen vorhandenen Datensatz mit demselben Namen löschen (falls vorhanden)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## Eine Kamera: einen Datensatz aufnehmen - Mac-Rechner

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

## Zwei Kameras: einen Datensatz aufnehmen - Mac-Rechner

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

## Während der Aufnahme

<grid>
<column width-ratio="0.508765">
![Das Bild zeigt die Terminaloberfläche während der Aufnahme eines Datensatzes mit OpenVSLAM auf einem Mac. Oben werden die Aufnahmeparameter angezeigt, etwa Auflösung, Bildrate und Encoder. Darunter steht das Aufnahmeprotokoll, das die Startzeit der Aufnahme, Versionsinformationen, die Thread-Anzahl und den Encoder festhält und auch den Aufnahmefortschritt zeigt, etwa 298/298 aufgenommene Episoden, insgesamt 5119.33 Sekunden. Unten gibt es Hinweise zur Taste „ESC", etwa sofort stoppen und den Datensatz hochladen. Dieses Bild gehört zum Arbeitsablauf der Datensatzaufnahme und veranschaulicht die Terminalrückmeldung während der Aufnahme.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![Dieses Bild zeigt die Kommandozeilen-Terminaloberfläche auf einem Mac, die zur Anzeige von Ausführungsprotokollinformationen zur Kamera-Datensatzaufnahme dient. Es enthält SVT-bezogene Konfigurationsparameter, etwa Konfigurationsparameter, die Version der Kodierungsbibliothek und die Werte der einzelnen Konfigurationselemente (wie Keyframe und CRF, Kodierungsauflösung), und zeigt außerdem Laufzeit-Statusprotokolle, etwa Meldungen zur MP4-Dateiverarbeitung, Aufzeichnungen von Gerätetrennungen und Zeitstempel während der Programmausführung. Insgesamt stellt es den Hintergrundbetriebszustand während der Kamera-Datensatzaufnahme dar.](../../en/images/d26-02.png)
</column>
</grid>

Tastatursteuerung über die Pfeiltasten:  
→ (Pfeil nach rechts) Die aktuelle Episode vorzeitig beenden; zur nächsten Episode übergehen.  
← (Pfeil nach links) Die aktuelle Episode verwerfen; erneut aufnehmen.  
ESC: sofort stoppen, das Video codieren und den Datensatz hochladen.

## Aufnahme abgeschlossen — Speicherverzeichnis des Datensatzes

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
