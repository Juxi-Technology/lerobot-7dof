[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | Português (PT)

<title>Ubuntu</title>

# Método 1: Verificar diretamente a partir da linha de comandos do Linux

## Verificar as portas dos dispositivos série

```Shell
ls /dev/ttyACM*
```

## Ligar as portas USB do computador e do braço do robô

Ligue primeiro o braço Follower e, em seguida, ligue o braço Leader

![A imagem mostra o processo de verificação das portas dos dispositivos série com a linha de comandos do Linux no Ubuntu. Mostra primeiro "nothing plugged in", depois, após ligar o braço Follower, aparece "/dev/ttyACM0"; a seguir, após ligar o braço Leader, aparece "/dev/ttyACM1". A imagem está estreitamente relacionada com o contexto, apresentando visualmente como as portas dos dispositivos série são vistas a partir da linha de comandos depois de ligar as portas USB do computador e do braço do robô, ajudando a explicar como verificar as portas dos dispositivos série a partir da linha de comandos do Linux.](../../en/images/d08-01.png)

# Método 2: A ferramenta oficial do LeRobot

```Shell
lerobot-find-port
```

![A imagem mostra a interface da linha de comandos para verificar as portas dos dispositivos série com a ferramenta oficial do LeRobot no Ubuntu. O comando é "lerobot-find-port", mostrando todas as portas disponíveis, incluindo várias portas "/dev/ttyACM". O prompt pede para desligar o cabo USB do Follower e encontrar o seu número de porta "/dev/ttyACM0", e depois voltar a ligar o cabo USB. Esta imagem está relacionada com o método dois para verificar as portas dos dispositivos série, apresentando visualmente os passos e os resultados.](../../en/images/d08-02.png)

![A imagem mostra o processo de utilização do comando `lerobot-find-port` para verificar as portas dos dispositivos série do braço do robô no Ubuntu. Após a execução do comando, este lista todas as portas disponíveis, depois pede para desligar o cabo USB do Leader e, por fim, mostra `/dev/ttyACM1` como o número da porta do dispositivo série do braço Leader. Esta imagem está relacionada com o método dois para verificar as portas dos dispositivos série, apresentando visualmente o resultado da obtenção dos números de porta com a ferramenta oficial do LeRobot.](../../en/images/d08-03.png)

# Registar as minhas portas

`/dev/ttyACM0` é o número da porta do dispositivo série do braço Follower

`/dev/ttyACM1` é o número da porta do dispositivo série do braço Leader

# Conceder permissões à porta

Conceder a todos os utilizadores permissão para ler e escrever nestes dispositivos série

```Shell
sudo chmod 666 /dev/ttyACM*
```
