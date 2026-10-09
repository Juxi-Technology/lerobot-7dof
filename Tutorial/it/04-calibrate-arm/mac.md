[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | Italiano | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Computer Mac

## Rivedere i numeri di porta

Braccio Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Braccio Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Calibrare il braccio Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![L'immagine mostra l'interfaccia a riga di comando per calibrare i servo SO101 su un Mac. Il comando è "lerobot-calibrate", con parametri tra cui robot.type, robot.port e robot.id. L'interfaccia visualizza le informazioni di configurazione del robot come "zihao_follower_arm". In basso, ti invita a premere "c" e Invio per avviare la calibrazione, e mostra anche messaggi come "zihao_follower_arm SO101Follower connected". Questa immagine corrisponde alla sezione "Calibrare il braccio Follower" e presenta il comando di calibrazione e il riscontro dell'interfaccia.](../../en/images/d13-01.png)

![L'immagine mostra l'interfaccia a riga di comando per un'operazione di calibrazione di LeRobot in Ubuntu. Il comando è "lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm", che visualizza le informazioni di calibrazione del Follower, tra cui la posizione minima, massima e corrente di ciascun giunto. I prompt operativi chiave sono evidenziati con riquadri rossi, come "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" e "press Enter to finish calibration", che riecheggiano i passaggi di calibrazione descritti nel contesto.](../../en/images/d13-02.png)

## Calibrare il braccio Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![L'immagine mostra l'interfaccia a riga di comando per una calibrazione di LeRobot in Ubuntu. La riga di comando ha eseguito operazioni come "sudo chmod 666 /dev/ttyACM*" e "lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1", visualizzando le informazioni sul numero di porta dei bracci Follower e Leader. L'interfaccia invita inoltre a premere Invio per avviare la calibrazione, a far percorrere a ogni giunto in sequenza i suoi limiti superiore e inferiore, e a premere Invio per terminare, e infine mostra il percorso in cui viene salvato il file di configurazione della calibrazione. Questa immagine è collegata al contenuto sulla calibrazione di LeRobot e presenta i passaggi di calibrazione.](../../en/images/d13-03.png)

## Visualizzare il file di configurazione della calibrazione

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## Bug comuni

- Uno o più servo non vengono trovati

![L'immagine mostra le informazioni sui parametri dei servo visualizzate durante la calibrazione del SO Follower. In alto sono mostrate le informazioni di connessione e un prompt di calibrazione che chiede di portare il Follower al centro del suo range di movimento e premere ENTER, quindi di far percorrere a tutti i giunti il loro range di movimento in ordine, registrare le posizioni e premere ENTER per fermarsi. La tabella sottostante elenca i valori NAME, MIN, POS e MAX per servo come shoulder_pan, shoulder_lift, elbow_flex, wrist_flex e gripper. Questa immagine è collegata alla calibrazione del braccio Follower e presenta i parametri durante la calibrazione.](../../en/images/d13-04.png)



## Note

### ① Un braccio smette di muoversi dopo aver raggiunto un limite

È necessario ricalibrarlo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servo non trovati

![L'immagine mostra l'interfaccia del terminale del Mac con un messaggio di errore derivante dall'esecuzione del codice robot di LeRobot. L'errore indica che il controllo dei motori di FeetechMotorsBus è fallito sulla porta '/dev/tty.usbmodem5AAF2193061', con gli ID motore da -1 a -6 mancanti e un modello previsto di 777. Elenca inoltre l'elenco completo dei motori previsti e l'elenco completo dei motori trovati. Questa immagine è collegata alla sezione "Bug comuni" e presenta come si manifesta come errore di runtime il problema "servo non trovati".](../../en/images/d13-05.png)

L'alimentazione dei servo non è collegata
