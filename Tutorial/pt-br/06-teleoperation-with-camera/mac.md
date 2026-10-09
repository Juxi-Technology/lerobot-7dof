[English](../en/06-teleoperation-with-camera/mac.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/mac.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/mac.md) | [Deutsch](../de/06-teleoperation-with-camera/mac.md) | [Español](../es/06-teleoperation-with-camera/mac.md) | [Français](../fr/06-teleoperation-with-camera/mac.md) | [Italiano](../it/06-teleoperation-with-camera/mac.md) | [日本語](../ja/06-teleoperation-with-camera/mac.md) | [한국어](../ko/06-teleoperation-with-camera/mac.md) | Português (BR) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/mac.md)

# Computador Mac

## Conectar a Câmera ao Computador

```Shell
lerobot-find-cameras opencv
```

![A imagem mostra o resultado da detecção após conectar uma câmera a um Mac. Na tela são listadas as informações de duas câmeras, que são a câmera externa e a câmera frontal integrada do Mac. O Fps da câmera externa é 60.00024 e o Fps da câmera integrada é 30.0. Esta imagem está relacionada ao conteúdo sobre conectar uma câmera a um Mac, apresentando visualmente a situação da detecção da câmera após a conexão e ajudando os usuários a entender o tipo, o ID, a API de backend e outras informações das câmeras, bem como a taxa de quadros de cada câmera.](../../en/images/d21-01.png)

## Uma Câmera, Teleoperação com o Feed da Câmera Exibido

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

Após a execução, a teleoperação começa

A janela do rerun.io abre, mostrando em tempo real a trajetória de cada articulação dos servos, juntamente com o feed ao vivo da câmera

e salva as imagens no diretório `~/username/outputs/captured_images`

![Esta imagem da interface do rerun.io corresponde ao conteúdo sobre a função de teleoperação com câmera no documento e é a interface de exibição em tempo real no cenário de teleoperação com uma única câmera. No lado esquerdo da interface há uma área de gráficos de linha, mostrando as curvas de variação da trajetória de cada articulação dos servos, com legendas marcadas com cores diferentes para distinguir os diversos dados de articulação; no lado direito há duas janelas, uma acima da outra, que mostram, respectivamente, o feed ao vivo da cena interna capturada pela câmera; na parte inferior há a área de linha do tempo e de informações de stream. Ao mesmo tempo, este cenário permite salvar as imagens capturadas pela câmera no diretório especificado `~/username/outputs/captured_images`.](../../en/images/d21-02.png)

## Várias Câmeras, Teleoperação com os Feeds das Câmeras Exibidos

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
