[English](../en/05-teleoperation/ubuntu.md) | [简体中文](../zh-hans/05-teleoperation/ubuntu.md) | [繁體中文](../zh-hant/05-teleoperation/ubuntu.md) | [Deutsch](../de/05-teleoperation/ubuntu.md) | [Español](../es/05-teleoperation/ubuntu.md) | [Français](../fr/05-teleoperation/ubuntu.md) | Italiano | [日本語](../ja/05-teleoperation/ubuntu.md) | [한국어](../ko/05-teleoperation/ubuntu.md) | [Português (BR)](../pt-br/05-teleoperation/ubuntu.md) | [Português (PT)](../pt-pt/05-teleoperation/ubuntu.md)

# Computer Ubuntu

## Concedere le autorizzazioni alla porta

Concedi a tutti gli utenti il permesso di leggere e scrivere su questi dispositivi seriali

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Teleoperazione

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![L'immagine mostra l'interfaccia su un computer Ubuntu per concedere le autorizzazioni alla porta e per la teleoperazione. Innanzitutto viene eseguito il comando `sudo chmod 666 /dev/ttyACM*` per concedere le autorizzazioni ai dispositivi seriali. Poi viene inserito il comando `lerobot-teleoperate`, che mostra le informazioni sul robot e sul teleop, come l'id del robot "zihao follower arm" e la porta "/dev/ttyACM0", e l'id del teleop "zihao leader arm" e la porta "/dev/ttyACM1". Questa immagine è strettamente correlata al contenuto sulla concessione delle autorizzazioni alla porta e sulla teleoperazione e presenta l'operazione e il suo risultato.](../../en/images/d16-01.png)
