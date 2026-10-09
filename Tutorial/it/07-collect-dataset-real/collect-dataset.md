[English](../en/07-collect-dataset-real/collect-dataset.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset.md) | [Español](../es/07-collect-dataset-real/collect-dataset.md) | [Français](../fr/07-collect-dataset-real/collect-dataset.md) | Italiano | [日本語](../ja/07-collect-dataset-real/collect-dataset.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset.md)

# Raccogliere un dataset per dimostrazione

## Eliminare qualsiasi dataset esistente con lo stesso nome (se presente)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```

## Una telecamera, raccolta di un dataset - computer Mac

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

## Due telecamere, raccolta di un dataset - computer Mac

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

## Durante la raccolta

<grid>
<column width-ratio="0.508765">
![L'immagine mostra l'interfaccia del terminale durante la raccolta di un dataset con OpenVSLAM su un Mac. In alto mostra i parametri di raccolta, come risoluzione, frequenza di fotogrammi e codificatore. Sotto c'è il log di raccolta, che registra l'ora di inizio della raccolta, le informazioni di versione, il numero di thread e il codificatore, e mostra anche l'avanzamento della raccolta, come 298/298 episodi raccolti, 5119.33 secondi in totale. In basso ci sono le note per il tasto "ESC", come fermare immediatamente e caricare il dataset. Questa immagine è collegata al flusso di lavoro della raccolta del dataset e presenta il riscontro del terminale durante la raccolta.](../../en/images/d26-01.png)
</column>
<column width-ratio="0.491235">
![Questa immagine mostra l'interfaccia del terminale a riga di comando su un Mac, usata per visualizzare le informazioni di log di esecuzione relative alla raccolta del dataset dalla telecamera. Contiene parametri di configurazione relativi a SVT, come parametri di config, la versione della libreria di codifica e i valori di ciascuna voce di configurazione (come key frame e CRF, risoluzione di codifica), e mostra anche i log di stato di runtime, come messaggi sull'elaborazione dei file MP4, registrazioni di disconnessione dei dispositivi e timestamp durante l'esecuzione del programma. Nel complesso presenta lo stato di esecuzione in background durante la raccolta del dataset dalla telecamera.](../../en/images/d26-02.png)
</column>
</grid>

Controlli con i tasti freccia della tastiera:  
→ (Freccia destra) Termina anticipatamente l'episodio corrente; passa all'episodio successivo.  
← (Freccia sinistra) Annulla l'episodio corrente; registralo di nuovo.  
ESC, ferma immediatamente, codifica il video e carica il dataset.

## Raccolta terminata — Directory di salvataggio del dataset

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a
```




## Stretta di mano

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
