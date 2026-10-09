[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | Italiano | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# Bug comuni e soluzioni

## La cattura della telecamera fallisce

![L'immagine mostra l'output del terminale quando viene eseguito il codice del robot LeRobot. Tra i log, quelli `INFO` mostrano l'apertura della telecamera OpenCV e la disconnessione del Follower; il log `ERROR` segnala che nel file `camera_opencv.py` la funzione `read` ha sollevato un `RuntimeError` a causa di `OpenCVCamera(0) read failed`. Questa immagine è collegata al problema "La cattura della telecamera fallisce" del documento e mostra visivamente l'errore che compare durante l'esecuzione del codice, aiutando a comprendere come si manifesta il problema in pratica.](../../en/images/d55-01.png)

Controlla se il cablaggio della telecamera del polso è allentato, soprattutto l'estremità vicino alla telecamera — quel connettore è molto soggetto a falsi contatti

## La telecamera si disconnette

![L'immagine mostra l'interfaccia con il codice e il messaggio di errore del problema di cattura fallita della telecamera che si verifica durante l'esecuzione di LeRobot HF. In alto compare la segnalazione del fallimento di connessione della telecamera OpenCV, in basso c'è lo stack di errore della funzione `record_loop` nel file `lerobot_record.py`, che segnala un `TimeoutError` durante `async_read` e indica che la telecamera non ha ottenuto un fotogramma entro 200 ms. Questa immagine corrisponde al contesto del problema "La cattura della telecamera fallisce" e mostra visivamente come il problema si presenta a livello di codice.](../../en/images/d55-02.png)

Riavvia la riga di comando

## Problema di comunicazione con il servo 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![Questa immagine è un log di errore del terminale e corrisponde al contenuto relativo al problema di comunicazione con il servo 1 menzionato nel documento. Il log mostra il percorso specifico del file di codice, nel file `lerobot/src/lerobot/motors/motors_bus.py`, alle righe 1069 e 1181, registra il messaggio di errore `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`, con la causa dell'errore `There is no status packet!`, e presenta in modo completo i dettagli dell'errore del problema di comunicazione con il servo 1 citato nel documento.](../../en/images/fix-04.png)

La soluzione: cambia ogni `num_retry` nel codice di `lerobot/src/lerobot/motors/motors_bus.py` a 99, soprattutto quello sulla riga che genera l'errore

![L'immagine mostra il contenuto del file di codice `motors_bus.py` nel progetto LeRobot. È evidenziato il metodo `write` della classe `MotorsBusABC`, con la variabile `num_retry` impostata a `99`. Questa immagine è collegata alla sezione "Problema di comunicazione con il servo 1" del documento e corrisponde all'operazione menzionata nella soluzione di impostare a 99 ogni `num_retry` nel codice di `lerobot/src/lerobot/motors/motors_bus.py`, in particolare sulla parte di codice che corrisponde alla riga che genera l'errore.](../../en/images/d55-03.png)

## Problema di comunicazione con il servo 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![Questa immagine mostra un log di errore, appartenente al messaggio di errore corrispondente al "Problema di comunicazione con il servo 2" del documento: durante l'esecuzione del codice si verifica un errore chiamato ConnectionError, che si manifesta come Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries, con l'informazione di errore aggiuntiva There is no status packet; è inoltre indicato il percorso del codice che ha generato l'errore. Questo log di errore corrisponde completamente alla descrizione del "Problema di comunicazione con il servo 2" del documento ed è la registrazione reale di quel problema.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![L'immagine mostra l'interfaccia di esecuzione da riga di comando nel terminale zsh su macOS. Il terminale mostra diversi percorsi di file e contenuti di file di codice; nel file `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` è presente il messaggio di errore `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`. Questa immagine è collegata al contenuto del "Problema di comunicazione con il servo 2" del documento e mostra visivamente l'interfaccia del terminale al momento dell'errore.](../../en/images/fix-05.png)
</column>
</grid>

Soluzione: ricalibra il braccio robotico
