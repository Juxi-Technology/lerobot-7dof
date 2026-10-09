[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | Español | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# Método 1: consultar directamente desde la línea de comandos de Linux

## Consultar los puertos de dispositivos serie

```Shell
ls /dev/ttyACM*
```

## Conectar los puertos USB de la computadora y del brazo robótico

Primero conecta el brazo Follower y luego el brazo Leader

![La imagen muestra el proceso de consultar los puertos de dispositivos serie con la línea de comandos de Linux en Ubuntu. Primero muestra "nada conectado"; después de conectar el brazo Follower aparece "/dev/ttyACM0"; y luego, tras conectar el brazo Leader, aparece "/dev/ttyACM1". La imagen está estrechamente relacionada con el contexto y presenta visualmente cómo se ven los puertos de dispositivos serie desde la línea de comandos tras conectar los puertos USB de la computadora y del brazo robótico, lo que ayuda a explicar cómo consultar los puertos de dispositivos serie desde la línea de comandos de Linux.](../../en/images/d08-01.png)

# Método 2: la herramienta oficial de LeRobot

```Shell
lerobot-find-port
```

![La imagen muestra la interfaz de línea de comandos para consultar los puertos de dispositivos serie con la herramienta oficial de LeRobot en Ubuntu. El comando es "lerobot-find-port" y muestra todos los puertos disponibles, incluidos varios puertos "/dev/ttyACM". El mensaje te pide que desconectes el cable USB del Follower y localices su número de puerto "/dev/ttyACM0", y después que vuelvas a conectar el cable USB. Esta imagen se relaciona con el método dos para consultar los puertos de dispositivos serie y presenta visualmente los pasos y los resultados.](../../en/images/d08-02.png)

![La imagen muestra el proceso de usar el comando `lerobot-find-port` para consultar los puertos de dispositivos serie del brazo robótico en Ubuntu. Tras ejecutar el comando, se enumeran todos los puertos disponibles, luego se te pide que desconectes el cable USB del Leader y, por último, se muestra `/dev/ttyACM1` como el número de puerto de dispositivo serie del brazo Leader. Esta imagen se relaciona con el método dos para consultar los puertos de dispositivos serie y presenta visualmente el resultado de obtener los números de puerto con la herramienta oficial de LeRobot.](../../en/images/d08-03.png)

# Registrar mis puertos

`/dev/ttyACM0` es el número de puerto de dispositivo serie del brazo Follower

`/dev/ttyACM1` es el número de puerto de dispositivo serie del brazo Leader

# Otorgar permisos al puerto

Dar a todos los usuarios permiso para leer y escribir estos dispositivos serie

```Shell
sudo chmod 666 /dev/ttyACM*
```
