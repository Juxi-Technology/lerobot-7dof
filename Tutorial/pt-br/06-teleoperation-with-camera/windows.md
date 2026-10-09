[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | Português (BR) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Computador Windows

## Conectar a Câmera ao Computador

```Shell
lerobot-find-cameras opencv
```

![Esta imagem é a interface de linha de comando do Windows com erro, correspondendo ao cenário de erro "a câmera não conecta" mencionado no documento. A informação de erro exibida tem duas partes: um erro global "Camera index out of range" e um erro do script relacionado ao lerobot "Failed to connect or configure OpenCV camera 0". A interface também lista as informações das câmeras detectadas, num total de 2 dispositivos chamados OpenCV Camera, ambos usando o backend DSHOW, formato YUY2, largura 640, altura 480, taxa de quadros 0.0FPS. Este conteúdo de erro corresponde à situação descrita no documento, em que a câmera não consegue conectar, mas ainda abre normalmente ao trocar de câmera no Tencent Meeting.](../../en/images/d22-01.png)

## Teleoperação com o Feed da Câmera Exibido

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

A janela do rerun.io abre, mostrando em tempo real a trajetória de cada articulação dos servos, juntamente com o feed ao vivo da câmera

e salva as imagens no diretório `C:\Users\username\outputs\captured_images`

![A imagem mostra a janela do rerun.io, exibindo em tempo real as trajetórias de cada articulação dos servos e o feed ao vivo da câmera. No lado esquerdo está a interface de blueprint, com opções como "teleoperation". No meio está o gráfico de trajetória, mostrando dados de posição de articulações como "observation_wrist_rot.pos". No lado direito está o feed da câmera, mostrando a cena sob a perspectiva do robô. No canto superior direito é exibido "Waiting for data on rerun: http://127.0.0.1:9876/remote...", com informações da fonte de dados abaixo. Esta imagem está relacionada ao conteúdo que apresenta a janela do rerun.io exibindo o feed da câmera em tempo real, mostrando visualmente o efeito.](../../en/images/d22-02.png)

## Se Você Encontrar o Seguinte Erro

A câmera não conecta, mas trocar de câmera no Tencent Meeting ainda abre normalmente

![A imagem mostra a interface de linha de comando de um computador Windows, exibindo o resultado da detecção de câmeras. No topo aparece "Administrador: Prompt de Comando - C:\Users\40743\miniconda3\condabin\conda.bat activate leroobot". Abaixo, ela exibe informações de "Detected Cameras", listando "Camera #0" como OpenCV Camera @ 0, com Type OpenCV, Backend api DSHOW, Default stream profile e outros parâmetros, e também mostra mensagens de erro como "ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)". Esta imagem está relacionada ao contexto do problema de conexão da câmera no documento, mostrando visualmente as informações de erro relacionadas.](../../en/images/d22-03.png)

Modifique o arquivo `lerobot\src\lerobot\cameras\utils.py` para alterar o backend do OpenCV para `cv2.CAP_SHOW`

![Esta imagem mostra um trecho de código Python usado para modificar o arquivo `utils.py` da biblioteca lerobot, correspondendo à etapa de solução do erro de conexão da câmera no Windows descrita no documento. O código define a função `get_cv2_backend()`, que configura o backend do OpenCV para diferentes sistemas: quando o sistema é Windows, o originalmente comentado `return int(cv2.CAP_MSMF)` permanece comentado e o conteúdo atualmente destacado é `return int(cv2.CAP_DSHOW)`, com uma observação ao lado explicando que essa abordagem substitui o AVFOUNDATION; a função também configura o backend correspondente do OpenCV para macOS e Linux. É a modificação de código central para resolver o problema de conexão da câmera especificada.](../../en/images/d22-04.png)

> Este é um bug que nem o Doubao consegue resolver; é tudo porque a biblioteca lerobot está encapsulada de forma muito profunda, e é muito difícil para iniciantes depurar

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## Conectar Várias Câmeras, Teleoperação com os Feeds das Câmeras Exibidos

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
