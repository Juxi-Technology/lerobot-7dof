[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | Português (BR) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# Método 1: Verificar Diretamente pela Linha de Comando do Linux

## Verificar as Portas dos Dispositivos Seriais

```Shell
ls /dev/ttyACM*
```

## Conectar as Portas USB do Computador e do Braço Robótico

Conecte primeiro o braço Follower e, em seguida, o braço Leader

![A imagem mostra o processo de verificação das portas dos dispositivos seriais com a linha de comando do Linux no Ubuntu. Ela mostra primeiro "nada conectado"; depois, ao conectar o braço Follower, aparece "/dev/ttyACM0"; em seguida, ao conectar o braço Leader, aparece "/dev/ttyACM1". A imagem está intimamente relacionada ao contexto, apresentando visualmente como as portas dos dispositivos seriais são vistas a partir da linha de comando após conectar as portas USB do computador e do braço robótico, ajudando a explicar como verificar as portas dos dispositivos seriais pela linha de comando do Linux.](../../en/images/d08-01.png)

# Método 2: A Ferramenta Oficial do LeRobot

```Shell
lerobot-find-port
```

![A imagem mostra a interface de linha de comando para verificar as portas dos dispositivos seriais com a ferramenta oficial do LeRobot no Ubuntu. O comando é "lerobot-find-port", exibindo todas as portas disponíveis, incluindo várias portas "/dev/ttyACM". O prompt pede que você desconecte o cabo USB do Follower e encontre o número da porta dele "/dev/ttyACM0" e, em seguida, reconecte o cabo USB. Esta imagem está relacionada ao método dois de verificação das portas dos dispositivos seriais, apresentando visualmente as etapas e os resultados.](../../en/images/d08-02.png)

![A imagem mostra o processo de uso do comando `lerobot-find-port` para verificar as portas dos dispositivos seriais do braço robótico no Ubuntu. Após o comando ser executado, ele lista todas as portas disponíveis, depois pede que você desconecte o cabo USB do Leader e, por fim, mostra `/dev/ttyACM1` como o número da porta do dispositivo serial do braço Leader. Esta imagem está relacionada ao método dois de verificação das portas dos dispositivos seriais, apresentando visualmente o resultado da obtenção dos números de porta com a ferramenta oficial do LeRobot.](../../en/images/d08-03.png)

# Registrar Minhas Portas

`/dev/ttyACM0` é o número da porta do dispositivo serial do braço Follower

`/dev/ttyACM1` é o número da porta do dispositivo serial do braço Leader

# Conceder Permissões à Porta

Conceda a todos os usuários permissão de leitura e escrita nesses dispositivos seriais

```Shell
sudo chmod 666 /dev/ttyACM*
```
