[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | Italiano | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# Metodo 1: controllare direttamente dalla riga di comando di Linux

## Controllare le porte dei dispositivi seriali

```Shell
ls /dev/ttyACM*
```

## Collegare le porte USB del computer e del braccio robotico

Collega prima il braccio Follower, poi collega il braccio Leader

![L'immagine mostra il processo di controllo delle porte dei dispositivi seriali con la riga di comando di Linux in Ubuntu. Mostra innanzitutto "nothing plugged in", poi dopo aver collegato il braccio Follower compare "/dev/ttyACM0"; quindi dopo aver collegato il braccio Leader compare "/dev/ttyACM1". L'immagine è strettamente correlata al contesto e presenta come si vedono le porte dei dispositivi seriali dalla riga di comando dopo aver collegato le porte USB del computer e del braccio robotico, aiutando a spiegare come controllare le porte dei dispositivi seriali dalla riga di comando di Linux.](../../en/images/d08-01.png)

# Metodo 2: lo strumento ufficiale LeRobot

```Shell
lerobot-find-port
```

![L'immagine mostra l'interfaccia a riga di comando per il controllo delle porte dei dispositivi seriali con lo strumento ufficiale LeRobot in Ubuntu. Il comando è "lerobot-find-port", che visualizza tutte le porte disponibili, tra cui diverse porte "/dev/ttyACM". Il prompt chiede di scollegare il cavo USB del Follower e di trovarne il numero di porta "/dev/ttyACM0", quindi di ricollegare il cavo USB. Questa immagine è collegata al metodo due per il controllo delle porte dei dispositivi seriali e presenta i passaggi e i risultati.](../../en/images/d08-02.png)

![L'immagine mostra il processo di utilizzo del comando `lerobot-find-port` per controllare le porte dei dispositivi seriali del braccio robotico in Ubuntu. Dopo l'esecuzione del comando, elenca tutte le porte disponibili, poi chiede di scollegare il cavo USB del Leader, e infine mostra `/dev/ttyACM1` come numero di porta del dispositivo seriale del braccio Leader. Questa immagine è collegata al metodo due per il controllo delle porte dei dispositivi seriali e presenta il risultato dell'ottenimento dei numeri di porta con lo strumento ufficiale LeRobot.](../../en/images/d08-03.png)

# Annotare le mie porte

`/dev/ttyACM0` è il numero di porta del dispositivo seriale del braccio Follower

`/dev/ttyACM1` è il numero di porta del dispositivo seriale del braccio Leader

# Concedere le autorizzazioni alla porta

Concedi a tutti gli utenti il permesso di leggere e scrivere su questi dispositivi seriali

```Shell
sudo chmod 666 /dev/ttyACM*
```
