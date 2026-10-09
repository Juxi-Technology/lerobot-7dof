[English](../en/06-teleoperation-with-camera/ubuntu.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/ubuntu.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/ubuntu.md) | [Deutsch](../de/06-teleoperation-with-camera/ubuntu.md) | [Español](../es/06-teleoperation-with-camera/ubuntu.md) | [Français](../fr/06-teleoperation-with-camera/ubuntu.md) | [Italiano](../it/06-teleoperation-with-camera/ubuntu.md) | [日本語](../ja/06-teleoperation-with-camera/ubuntu.md) | [한국어](../ko/06-teleoperation-with-camera/ubuntu.md) | Português (BR) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/ubuntu.md)

# Computador Ubuntu

## Conectar a Câmera ao Computador

```Shell
lerobot-find-cameras opencv
```

![Esta imagem mostra o resultado da detecção de câmeras no terminal do Ubuntu. O comando "lerobot-find-cameras opencv" foi executado, detectando uma câmera numerada Camera #0 chamada OpenCV Camera com caminho /dev/video0, tipo OpenCV e API de backend V4L2; seus parâmetros de formato de fluxo padrão incluem formato Fourcc YUYV, largura 640, altura 480 e taxa de quadros 30.0. Por fim, mostra que o salvamento das imagens foi concluído e que as imagens foram armazenadas no diretório outputs/captured_images. Isso corresponde ao conteúdo sobre encontrar câmeras conectadas em um computador Ubuntu.](../../en/images/d20-01.png)

## Teleoperação com o Feed da Câmera Exibido

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

## Várias Câmeras, Teleoperação com os Feeds das Câmeras Exibidos

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
