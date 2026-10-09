[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | Italiano | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Computer Ubuntu

## Concedere le autorizzazioni alla porta

Concedi a tutti gli utenti il permesso di leggere e scrivere su questi dispositivi seriali

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Calibrare il braccio Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![L'immagine mostra l'interfaccia del terminale su un computer Ubuntu durante l'esecuzione del comando "lerobot-calibrate" per calibrare il braccio Follower. Visualizza le informazioni di connessione del Follower, i nomi dei giunti e i valori dei limiti superiore/inferiore. Le informazioni chiave comprendono: premere Invio per avviare la calibrazione, far percorrere a ogni giunto in sequenza i suoi limiti superiore e inferiore, premere Invio per terminare la calibrazione; e "Calibration saved to" e altre informazioni sul percorso del file di calibrazione. Questa immagine è strettamente correlata ai passaggi per calibrare il braccio Follower e presenta il riscontro del terminale durante la calibrazione.](../../en/images/d12-01.png)

## Calibrare il braccio Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![L'immagine mostra l'interfaccia su un computer Ubuntu dopo aver concesso le autorizzazioni alla porta. La riga di comando ha inserito "sudo chmod 666 /dev/ttyACM*", e dopo l'esecuzione ha visualizzato informazioni come "zihao_leader_arm". In basso ci sono prompt come "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" e "press Enter to finish calibration", insieme a "Calibration saved to" e altre informazioni sul percorso relative alla calibrazione. Questa immagine corrisponde alla sezione "Calibrare il braccio Leader" e presenta l'interfaccia di preparazione prima della calibrazione.](../../en/images/d12-02.png)

## Visualizzare il file di configurazione della calibrazione

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![L'immagine mostra il contenuto del file "zihao_follower_arm.json" visualizzato nel terminale di Ubuntu. Il file contiene le informazioni di configurazione di diversi bracci, come shoulder_pan, shoulder_lift, elbow_flex e wrist_flex, ciascun braccio con parametri come id, drive_mode, homing_offset, range_min e range_max. Questa immagine è collegata alla sezione "Visualizzare il file di configurazione della calibrazione" e presenta le informazioni specifiche sui parametri nel file di calibrazione, aiutando gli utenti a comprendere la configurazione di ciascun braccio.](../../en/images/d12-03.png)



## Note

### ① Un braccio smette di muoversi dopo aver raggiunto un limite

È necessario ricalibrarlo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servo non trovati

![Questo è uno screenshot che mostra un'interfaccia di errore nel terminale di Ubuntu, corrispondente alla nota "servo non trovati". L'interfaccia segnala un RuntimeError, nello specifico "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'", ovvero il controllo dei servo è fallito. Elenca inoltre le informazioni sui servo previsti, con gli ID motore previsti da 1 a 6 e il modello previsto 777, ma l'elenco dei motori effettivamente trovati è vuoto; insieme al contesto, questo errore è causato dall'alimentazione dei servo non inserita.](../../en/images/d12-04.png)

L'alimentazione dei servo non è collegata
