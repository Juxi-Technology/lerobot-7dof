[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | [Français](../fr/resources.md) | Italiano | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>Alcune risorse</title>

# Il [file URDF](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf) ufficiale di Lerbot



## URDF Studio

https://urdf.d-robotics.cc/



## Controllo della simulazione ROS2 (implementalo tu stesso)

https://github.com/holmsslk/so-arm-moveit-hardware



## L'interfaccia grafica ufficiale di LeRobot

https://github.com/huggingface/leLab

LeLab è una web app che riunisce l'intero flusso di lavoro di LeRobot — calibrazione, teleoperazione, registrazione, addestramento, riproduzione — in un'unica interfaccia nel browser. Basta collegare il braccio robotico, aprire l'app e puoi iniziare a lavorare. Nessun lavoro complicato da riga di comando e nessun input da tastiera necessario.

🤗 Il punto di accesso web nativo di LeRobot, pensato per portare i nuovi utenti da "pronto all'uso" ad "addestrare la propria prima policy" in pochi minuti.

🤗 Installa e avvia tutto con un solo comando.



# Controllare il braccio Follower da un telefono

https://huggingface.co/docs/lerobot/main/en/phone_teleop





## Sviluppo di robotica cloud: dispositivi ROS 2 e simulazione Isaac Sim LeRobot e streaming di dati su AWS

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## Impostare gli ID dei servo e la calibrazione del centro nella UI web

https://bambot.org/feetech.js?lang=zh

1. Inserisci 0 o 1 a seconda del modello di servo, poi fai clic su "Connect"

![L'immagine mostra l'interfaccia di connessione per impostare gli ID dei servo e la calibrazione del centro nella UI web. Nell'interfaccia è presente una sezione "Connect", che comprende un menu a tendina per il baud rate (1.000.000 bps, Index 0), un menu a tendina per il protocollo (0=STS/SMS, 1=SCS, attualmente selezionato 0), un pulsante di connessione (verde, contrassegnato dal numero 2) e un indicatore di stato (rosso, con la scritta "Disconnected"). Questa immagine richiama quanto detto sopra, "Inserisci 0 o 1 a seconda del modello di servo, poi fai clic su "Connect"", e mostra visivamente le impostazioni dell'interfaccia per l'operazione di connessione.](../en/images/d58-01.png)

2. Scansiona i servo con ID da 1\~6; puoi usare FOUND nei risultati della scansione per confermare il servo con l'ID corrispondente. Ad esempio, nell'immagine è stato trovato il servo con ID 1

![Questa è una schermata dell'interfaccia di scansione dei servo del software relativo a Lerbot; il contenuto principale è il processo e il risultato della scansione dei servo. L'interfaccia ha i campi di input "Starting ID" e "Ending ID", in cui sono inseriti rispettivamente 1 e 6, e un pulsante "Start scan", sotto il quale c'è l'area corrispondente per mostrare i risultati della scansione, che corrisponde al contenuto del documento relativo al servo con ID trovato. Il log dei risultati della scansione mostra che solo durante la scansione dell'ID1 è stato ottenuto con successo il servo, con "FOUND: Pos-1230, Mode=0, BaudIdx=0", mentre scansionando dall'ID2 all'ID6 compare ogni volta un errore di lettura anomala della posizione e di assenza di status packet.](../en/images/d58-02.png)

3. Impostazione dell'ID e calibrazione del centro

① Imposta l'ID del servo corrente sul valore dell'ID del servo scansionato

② Inserisci un numero in "ID management" e fai clic su "Change ID" per impostare l'ID

③ Calibrazione del centro (il centro del servo STS3215 è 2047, il centro del servo SCS0009 è 511)

Servo STS: inserisci 2047 in "Position control" e fai clic su "Set"

Servo SCS: inserisci 511 in "Position control" e fai clic su "Set"

![L'immagine mostra l'interfaccia di controllo di un singolo servo nell'URDF Studio ufficiale di Lerbot. Nell'interfaccia il "Current servo ID" è mostrato come 1; sotto, in "ID management", è possibile inserire un numero e fare clic su "Change ID"; nell'area "Position control" c'è un pulsante "Read position", che mostra la posizione 2047, accanto a un pulsante "Set". Questa immagine è strettamente legata al contesto e mostra visivamente i passaggi citati nel contesto, come impostare come ID del servo corrente l'ID del servo scansionato, inserire un numero in "ID management" e fare clic su "Change ID" per impostare l'ID, e inserire 2047 nel "Position control" e fare clic su "Set" per il servo STS.](../en/images/d58-03.png)
