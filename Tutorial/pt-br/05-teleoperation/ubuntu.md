[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | Português (BR) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Computador Ubuntu

## Conceder Permissões à Porta

Conceda a todos os usuários permissão de leitura e escrita nesses dispositivos seriais

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

![A imagem mostra a interface em um computador Ubuntu para conceder permissões à porta e para a teleoperação. Primeiro, o comando `sudo chmod 666 /dev/ttyACM*` é executado para conceder permissões aos dispositivos seriais. Em seguida, o comando `lerobot-teleoperate` é digitado, mostrando as informações do robô e do teleop, como o id do robô "zihao follower arm" e a porta "/dev/ttyACM0", e o id do teleop "zihao leader arm" e a porta "/dev/ttyACM1". Esta imagem está intimamente relacionada ao conteúdo sobre conceder permissões de porta e teleoperação, apresentando visualmente a operação e seu resultado.](../../en/images/d16-01.png)
