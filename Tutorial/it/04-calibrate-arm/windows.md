[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | Italiano | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Computer Windows



<callout emoji="🚫">
I bracci Leader e Follower devono essere entrambi collegati
</callout>

## Calibrare il braccio Follower

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![L'immagine mostra l'interfaccia a riga di comando su un computer Windows durante l'esecuzione di una calibrazione lerobot. Il comando è "lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm". L'interfaccia visualizza le informazioni di calibrazione, tra cui prompt come "zihao_follower_arm SO10IFollower connected", ed elenca anche i valori NAME, MIN, POS e MAX di ciascun giunto del braccio robotico. Durante la calibrazione, invita l'utente a portare il braccio robotico al centro del suo range di movimento e premere ENTER, registrando le posizioni, e a premere ENTER per fermarsi. Questa immagine è collegata alla calibrazione del braccio Follower e mostra i passaggi specifici e il riscontro dell'interfaccia.](../../en/images/d14-01.png)

## Calibrare il braccio Leader

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![L'immagine mostra l'interfaccia a riga di comando che calibra il braccio robotico con il comando lerobot-calibrate su un computer Windows. Visualizza le informazioni sulla calibrazione dei bracci Follower e Leader, tra cui il percorso di salvataggio della posizione di calibrazione, il tipo di robot, il numero di porta e l'ID. Invita inoltre a portare il Follower al centro del suo range di movimento e premere ENTER, a far percorrere a ciascun giunto l'intero range di movimento, a registrare le posizioni e a premere ENTER per fermarsi. In basso mostra il nome, il valore minimo, la posizione corrente e il valore massimo di ciascun giunto.](../../en/images/d14-02.png)

## Dove vengono esportati i file

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## Calibrare un braccio robotico diverso

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## Note

### ① Un braccio smette di muoversi dopo aver raggiunto un limite

È necessario ricalibrarlo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servo non trovati

![L'immagine mostra un messaggio di errore durante l'esecuzione del programma lerobot in macOS. Mentre il programma è in esecuzione, compare un RuntimeError: "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'", che indica ID servo mancanti, tra cui i servo da 1 a 6, tutti con un numero di modello previsto di 777, ma l'elenco dei servo effettivamente trovati è vuoto. Questo è collegato alla nota "servo non trovati"; potrebbe dipendere dal fatto che i servo non sono collegati, quindi ricollegali e ruota il connettore.](../../en/images/d14-03.png)

L'alimentazione dei servo non è collegata; ricollegala e ruota il connettore
