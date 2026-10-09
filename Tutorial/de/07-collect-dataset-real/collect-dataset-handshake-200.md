[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | Deutsch | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Italiano](../it/07-collect-dataset-real/collect-dataset-handshake-200.md) | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# Einen Datensatz durch Vorführen aufnehmen — Handshake 200

## Ein Dataset-Repo auf HuggingFace erstellen

https://huggingface.co/new-dataset

![Das Bild zeigt die Oberfläche zum Erstellen eines neuen Datensatz-Repositories auf HuggingFace. „Owner" wird als TommyZihao angezeigt, im Eingabefeld „Dataset name" steht lerobot_zihao_dataset_shake200, „License" ist auf mit gesetzt, und die Option „Public" ist ausgewählt; darunter steht, dass jeder diesen Datensatz ansehen kann, aber nur der Datensatzeigentümer oder Organisationsmitglieder committen können. Unten wird darauf hingewiesen, dass Sie den Datensatz nach dem Erstellen über die Weboberfläche oder git hochladen können, und es gibt eine Schaltfläche „Create dataset". Dieses Bild gehört zum Inhalt über das Erstellen eines Dataset-Repos auf HuggingFace und zeigt die Oberfläche für den Vorgang des Erstellens eines Datensatz-Repositories.](../../en/images/d27-01.png)

## Einen vorhandenen Datensatz mit demselben Namen löschen (falls vorhanden)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Datensatzaufnahme Shake200

Eine Kamera: einen Datensatz aufnehmen - Mac-Rechner

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

## Während der Aufnahme

<grid>
<column width-ratio="0.508765">
![Dies ist die Terminaloberfläche einer Bildschirmaufnahme während der Aufnahme des Handshake-Datensatzes (Shake200). Sowohl oben als auch leicht unterhalb der Mitte der Oberfläche steht die mit „INFO 202-04-15 17:00:22.227 Recording episode 36" gekennzeichnete Information, die anzeigt, dass gerade der 36. Datenerfassungszyklus (Episode) aufgezeichnet wird; außerdem werden die relevanten Parameter des SVT-AV1-Encoders und die Fortschrittsinformation „200/200 \[00:00:00::00, 4911.09 examples/s\]" dargestellt. Das zeigt, dass sich diese Phase mitten in der Datensatzaufnahme befindet, entspricht dem im Dokument beschriebenen Schritt „Während der Aufnahme" und veranschaulicht den Systemzustand während der Aufnahme.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![Dieses Bild ist die Laufzeitoberfläche des Mac-Terminals und zeigt die Konfigurations- und Laufzeitprotokollinformationen des SVT-Projekts. Die Oberfläche enthält mehrere Konfigurationsparameter, etwa die GOP-Größe, die Mini-Pod-Größe, den Keyframe-Typ und den CRF-Wert der SVT-Konfiguration, sowie Versionsinformationen der SVT-A1-Encoder-Bibliothek. Die Logs in der unteren Hälfte protokollieren Vorgänge rund um Videodateien, erwähnen das Verschieben des Atoms an den Anfang der Datei sowie den Verbindungs- und Trennungsstatus mehrerer Komponenten wie OpenVCamera und SV280 follower und zeigen schließlich die Abschlussmeldung der Terminalausführung im aktuellen Pfad. Das entspricht dem Terminal-Laufszenario während der Vorführ-Aufnahme des Handshake-200-Datensatzes.](../../en/images/d27-02.png)
</column>
</grid>

Tastatursteuerung über die Pfeiltasten:  
→ (Pfeil nach rechts) Die aktuelle Episode vorzeitig beenden; zur nächsten Episode übergehen.  
← (Pfeil nach links) Die aktuelle Episode verwerfen; erneut aufnehmen.  
ESC: sofort stoppen, das Video codieren und den Datensatz hochladen.

## Aufnahme abgeschlossen — Speicherverzeichnis des Datensatzes

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
