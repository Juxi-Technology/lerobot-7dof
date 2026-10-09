[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/ubuntu.md) | Português (PT)

# Computador Ubuntu

## Ligar a câmara ao computador

```Shell
lerobot-find-cameras opencv
```

![Esta imagem mostra o resultado da deteção de câmaras no terminal do Ubuntu. Foi executado o comando "lerobot-find-cameras opencv", detetando uma câmara numerada Camera #0 com o nome OpenCV Camera, com o caminho /dev/video0, tipo OpenCV e API de backend V4L2; os seus parâmetros predefinidos de formato de fluxo incluem o formato Fourcc YUYV, largura 640, altura 480 e taxa de fotogramas 30.0. Por fim mostra que a gravação de imagens está concluída e que as imagens foram armazenadas no diretório outputs/captured_images. Isto corresponde ao conteúdo sobre encontrar câmaras ligadas num computador Ubuntu.](../../en/images/d20-01.png)

## Teleoperação com a imagem da câmara apresentada

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```

## Várias câmaras, teleoperação com as imagens das câmaras apresentadas

```Shell
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm \
    --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30, fourcc: "MJPG"}}" \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm \
    --display_data=true
```
