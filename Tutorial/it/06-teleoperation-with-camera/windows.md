[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | Italiano | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Computer Windows

## Collegare la telecamera al computer

```Shell
lerobot-find-cameras opencv
```

![Questa immagine mostra l'interfaccia da riga di comando di Windows con un errore: corrisponde allo scenario di errore "la telecamera non riesce a connettersi" menzionato nel documento. Le informazioni di errore mostrate si dividono in due parti: un errore globale "Camera index out of range" e l'errore dello script relativo a lerobot "Failed to connect or configure OpenCV camera 0". L'interfaccia elenca anche le telecamere rilevate: in totale 2 dispositivi chiamati OpenCV Camera, entrambi con backend DSHOW, formato YUY2, larghezza 640, altezza 480 e frequenza di fotogrammi 0.0FPS. Questo contenuto di errore corrisponde alla situazione descritta nel documento in cui la telecamera non riesce a connettersi, ma funziona comunque normalmente se si cambia telecamera in Tencent Meeting.](../../en/images/d22-01.png)

## Teleoperazione con il feed della telecamera visualizzato

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

Si apre la finestra di rerun.io, che mostra in tempo reale la traiettoria di ciascun giunto dei servo, insieme al feed live della telecamera

e salva le immagini nella directory `C:\Users\username\outputs\captured_images`

![L'immagine mostra la finestra di rerun.io, che visualizza in tempo reale la traiettoria di ciascun giunto dei servo e il feed live della telecamera. Sul lato sinistro c'è l'interfaccia blueprint con opzioni come "teleoperation". Al centro c'è il grafico delle traiettorie, che mostra i dati di posizione dei giunti come "observation_wrist_rot.pos". A destra c'è il feed della telecamera, che mostra la scena dal punto di vista del robot. In alto a destra mostra "Waiting for data on rerun: http://127.0.0.1:9876/remote...", con sotto le informazioni sull'origine dei dati. Questa immagine è collegata al contenuto che descrive la finestra di rerun.io che mostra in tempo reale il feed della telecamera e ne presenta l'effetto.](../../en/images/d22-02.png)

## Se incontri il seguente errore

La telecamera non riesce a connettersi, ma cambiando telecamera in Tencent Meeting si apre comunque normalmente

![L'immagine mostra l'interfaccia da riga di comando di Windows con il risultato del rilevamento delle telecamere. In alto compare la scritta "Amministratore: Prompt dei comandi - C:\\Users\\40743\\miniconda3\\condabin\\conda.bat activate leroobot". Sotto viene stampata l'informazione "Detected Cameras", che elenca "Camera #0" come OpenCV Camera @ 0, con Type OpenCV, Backend api DSHOW, Default stream profile e altri parametri; compaiono inoltre messaggi di errore come "ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)". Questa immagine è collegata al contesto del problema di connessione della telecamera descritto nel documento e presenta visivamente le informazioni di errore pertinenti.](../../en/images/d22-03.png)

Modifica il file `lerobot\src\lerobot\cameras\utils.py` per cambiare il backend OpenCV in `cv2.CAP_SHOW`

![Questa immagine mostra un frammento di codice Python, cioè una parte del file `utils.py` della libreria lerobot, che corrisponde al passaggio di risoluzione dell'errore di connessione della telecamera su Windows descritto nel documento. Il codice definisce la funzione `get_cv2_backend()`, che imposta il backend OpenCV in base al sistema operativo: quando il sistema è Windows, la riga commentata `return int(cv2.CAP_MSMF)` viene disattivata e il contenuto attualmente evidenziato è `return int(cv2.CAP_DSHOW)`, con un'annotazione a fianco che spiega che questa soluzione sostituisce AVFOUNDATION; la funzione imposta inoltre il backend OpenCV corrispondente per macOS e Linux. È il contenuto centrale di modifica del codice che risolve il problema di connessione della telecamera indicata.](../../en/images/d22-04.png)

> Questo è un bug che nemmeno Doubao riesce a risolvere; tutto perché la libreria lerobot è incapsulata in modo troppo profondo, ed è molto difficile per i principianti fare il debug

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## Collegare più telecamere, teleoperazione con i feed delle telecamere visualizzati

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
