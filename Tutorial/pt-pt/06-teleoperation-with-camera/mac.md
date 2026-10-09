[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/mac.md) | Português (PT)

# Computador Mac

## Ligar a câmara ao computador

```Shell
lerobot-find-cameras opencv
```

![A imagem mostra o resultado da deteção após ligar uma câmara a um Mac. Lista a informação de duas câmaras: a câmara externa e a câmara frontal integrada do Mac. O Fps da câmara externa é 60.00024 e o Fps da câmara integrada é 30.0. Esta imagem está relacionada com o conteúdo sobre ligar uma câmara a um Mac, apresentando visualmente o resultado da deteção após a ligação e ajudando os utilizadores a compreender o tipo, o ID, a API de backend e outra informação de cada câmara, bem como a taxa de fotogramas de cada uma.](../../en/images/d21-01.png)

## Uma câmara, teleoperação com a imagem da câmara apresentada

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

Após a execução, a teleoperação inicia

A janela do rerun.io abre-se, mostrando a trajetória de cada articulação dos servos em tempo real, juntamente com a imagem da câmara ao vivo

e guarda as imagens no diretório `~/username/outputs/captured_images`

![A imagem mostra a interface do rerun.io, correspondendo ao conteúdo sobre a função de teleoperação com câmara no documento, e é a interface de visualização em tempo real do cenário de teleoperação com uma única câmara. À esquerda está uma área de gráficos de linhas, mostrando as curvas de trajetória de cada articulação dos servos, com legendas assinaladas em cores diferentes para distinguir os vários dados das articulações; à direita há duas janelas sobrepostas, mostrando a imagem interior captada em tempo real pelas câmaras; na parte inferior está a área da linha temporal e da informação de fluxo. Este cenário também permite guardar as imagens captadas pela câmara no diretório especificado `~/username/outputs/captured_images`.](../../en/images/d21-02.png)

## Várias câmaras, teleoperação com as imagens das câmaras apresentadas

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
