[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | Português (PT)

# Computador Ubuntu

## Conceder permissões à porta

Conceder a todos os utilizadores permissão para ler e escrever nestes dispositivos série

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Teleoperação

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![A imagem mostra a interface num computador Ubuntu para conceder permissões à porta e para a teleoperação. Primeiro é executado o comando `sudo chmod 666 /dev/ttyACM*` para conceder permissões aos dispositivos série. Depois é introduzido o comando `lerobot-teleoperate`, mostrando a informação do robô e do teleop, como o id do robô "zihao follower arm" e a porta "/dev/ttyACM0", e o id do teleop "zihao leader arm" e a porta "/dev/ttyACM1". Esta imagem está estreitamente relacionada com o conteúdo sobre conceder permissões à porta e a teleoperação, apresentando visualmente a operação e o seu resultado.](../../en/images/d16-01.png)
