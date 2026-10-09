[English](../en/07-collect-dataset-real/collect-dataset-handshake-200.md) | [简体中文](../zh-hans/07-collect-dataset-real/collect-dataset-handshake-200.md) | [繁體中文](../zh-hant/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Deutsch](../de/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Español](../es/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Français](../fr/07-collect-dataset-real/collect-dataset-handshake-200.md) | Italiano | [日本語](../ja/07-collect-dataset-real/collect-dataset-handshake-200.md) | [한국어](../ko/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (BR)](../pt-br/07-collect-dataset-real/collect-dataset-handshake-200.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/collect-dataset-handshake-200.md)

# Raccogliere un dataset per dimostrazione — Handshake 200

## Creare un repository di dataset su HuggingFace

https://huggingface.co/new-dataset

![L'immagine mostra l'interfaccia per creare un nuovo repository di dataset su HuggingFace. Nel campo "Owner" è indicato TommyZihao, nel campo "Dataset name" è inserito lerobot_zihao_dataset_shake200, "License" è impostata su mit e l'opzione "Public" è selezionata; la nota sottostante spiega che chiunque può visualizzare questo dataset, mentre solo il proprietario del dataset o i membri dell'organizzazione possono inviare file. In fondo si ricorda che, dopo aver creato il dataset, è possibile caricare i file tramite l'interfaccia web o git, ed è presente il pulsante "Create dataset". Questa immagine è collegata al contenuto sulla creazione di un repository di dataset su HuggingFace ed è la schermata dell'operazione di creazione del repository del dataset.](../../en/images/d27-01.png)

## Eliminare qualsiasi dataset esistente con lo stesso nome (se presente)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```

## Raccolta del dataset Shake200

Una telecamera, raccolta di un dataset - computer Mac

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

## Durante la raccolta

<grid>
<column width-ratio="0.508765">
![Questo è il contenuto dell'interfaccia del terminale durante la registrazione del dataset di handshake (Shake200). Sia nella parte superiore sia in quella centrale inferiore dell'interfaccia compare l'informazione "INFO 202-04-15 17:00:22.227 Recording episode 36", che mostra la registrazione del 36° episodio di raccolta dati, insieme ai parametri relativi al codificatore SVT-AV1 e all'informazione di avanzamento "200/200 \[00:00:00::00, 4911.09 examples/s\]". Questo riflette che in questa fase è in corso la raccolta del dataset, corrisponde alla sezione "Durante la raccolta" descritta nel documento e mostra lo stato di esecuzione del sistema al momento della raccolta.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.491235">
![Questa immagine è l'interfaccia di esecuzione del terminale su Mac, che mostra la configurazione del progetto SVT e le informazioni di log. L'interfaccia contiene diversi parametri di configurazione, come la dimensione GOP, la dimensione mini-pod, il tipo di keyframe e il valore CRF della configurazione SVT, oltre alle informazioni sulla versione della libreria SVT-A1 Encoder. La parte inferiore dell'interfaccia registra contenuti relativi alle operazioni sui file video, citando lo spostamento dell'atomo all'inizio del file e lo stato di connessione e disconnessione di diversi componenti come OpenVCamera e SV280 follower; infine mostra il messaggio di fine esecuzione del terminale nel percorso corrente, corrispondente allo scenario di esecuzione del terminale durante la raccolta per dimostrazione del dataset handshake 200.](../../en/images/d27-02.png)
</column>
</grid>

Controlli con i tasti freccia della tastiera:  
→ (Freccia destra) Termina anticipatamente l'episodio corrente; passa all'episodio successivo.  
← (Freccia sinistra) Annulla l'episodio corrente; registralo di nuovo.  
ESC, ferma immediatamente, codifica il video e carica il dataset.

## Raccolta terminata — Directory di salvataggio del dataset

```Shell
/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_shake200
```
