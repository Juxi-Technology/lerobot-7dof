[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | Español | [Français](../fr/05-teleoperation/ubuntu.md) | [Italiano](../it/05-teleoperation/ubuntu.md) | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Computadora con Ubuntu

## Otorgar permisos al puerto

Dar a todos los usuarios permiso para leer y escribir estos dispositivos serie

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Teleoperación

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![La imagen muestra la interfaz en una computadora con Ubuntu para otorgar permisos al puerto y para la teleoperación. Primero se ejecuta el comando `sudo chmod 666 /dev/ttyACM*` para otorgar permisos a los dispositivos serie. Después se introduce el comando `lerobot-teleoperate`, que muestra la información del robot y del teleop, como el id del robot "zihao follower arm" y el puerto "/dev/ttyACM0", y el id del teleop "zihao leader arm" y el puerto "/dev/ttyACM1". Esta imagen está estrechamente relacionada con el contenido sobre otorgar permisos al puerto y la teleoperación, y presenta visualmente la operación y su resultado.](../../en/images/d16-01.png)
