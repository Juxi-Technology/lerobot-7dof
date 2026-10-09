[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | Italiano | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Computer Mac

## Collegare la telecamera al computer

```Shell
lerobot-find-cameras opencv
```

![L'immagine mostra il risultato del rilevamento dopo aver collegato una telecamera a un Mac. Nella schermata sono elencate le informazioni di due telecamere: la telecamera esterna e la webcam frontale integrata del Mac. La Fps della telecamera esterna è 60.00024, mentre la Fps della webcam integrata è 30.0. Questa immagine è collegata al contenuto che descrive il collegamento di una telecamera a un Mac e presenta visivamente il risultato del rilevamento dopo la connessione, aiutando l'utente a comprendere il tipo, l'ID, l'API di backend e la frequenza di fotogrammi di ciascuna telecamera.](../../en/images/d21-01.png)

## Una telecamera, teleoperazione con il feed della telecamera visualizzato

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

Dopo l'esecuzione, inizia la teleoperazione

Si apre la finestra di rerun.io, che mostra in tempo reale la traiettoria di ciascun giunto dei servo, insieme al feed live della telecamera

e salva le immagini nella directory `~/username/outputs/captured_images`

![Questa immagine mostra l'interfaccia di rerun.io e corrisponde al contenuto sulle funzioni di teleoperazione con telecamera; è l'interfaccia di visualizzazione in tempo reale nello scenario di teleoperazione con una sola telecamera. Sul lato sinistro dell'interfaccia c'è un'area con grafici a linee, che mostra le curve di variazione della traiettoria di ciascun giunto dei servo, con una legenda di colori diversi per distinguere i vari dati dei giunti; sul lato destro ci sono due finestre, una sopra l'altra, che mostrano ciascuna il feed in tempo reale della scena interna ripresa dalla telecamera; in basso c'è l'area della timeline e delle informazioni sugli stream, e allo stesso tempo questo scenario supporta il salvataggio delle immagini catturate dalla telecamera nella directory specificata `~/username/outputs/captured_images`.](../../en/images/d21-02.png)

## Più telecamere, teleoperazione con i feed delle telecamere visualizzati

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
