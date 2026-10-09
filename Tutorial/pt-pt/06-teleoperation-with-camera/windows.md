[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | Português (PT)

# Computador Windows

## Ligar a câmara ao computador

```Shell
lerobot-find-cameras opencv
```

![Esta imagem é o ecrã de erro da linha de comandos do Windows, correspondendo ao cenário de erro "a câmara não consegue ligar-se" mencionado no documento. A informação de erro apresentada tem duas partes: um erro global "Camera index out of range" e um erro do script relacionado com o lerobot "Failed to connect or configure OpenCV camera 0". A interface lista também a informação das câmaras detetadas: 2 dispositivos com o nome OpenCV Camera, ambos com backend DSHOW, formato YUY2, largura 640, altura 480 e taxa de fotogramas 0.0FPS. Este conteúdo de erro corresponde à situação descrita no documento, em que a câmara não consegue ligar-se mas continua a abrir normalmente quando se troca de câmara no Tencent Meeting.](../../en/images/d22-01.png)

## Teleoperação com a imagem da câmara apresentada

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

A janela do rerun.io abre-se, mostrando a trajetória de cada articulação dos servos em tempo real, juntamente com a imagem da câmara ao vivo

e guarda as imagens no diretório `C:\Users\username\outputs\captured_images`

![A imagem mostra a janela do rerun.io, apresentando as trajetórias das articulações dos servos e a imagem da câmara ao vivo em tempo real. À esquerda está a interface de blueprint com opções como "teleoperation". Ao centro está o gráfico de trajetórias, mostrando dados de posição das articulações como "observation_wrist_rot.pos". À direita está a imagem da câmara, mostrando a cena a partir da perspetiva do robô. No canto superior direito mostra "Waiting for data on rerun: http://127.0.0.1:9876/remote...", com informação da fonte de dados em baixo. Esta imagem está relacionada com o conteúdo que descreve a janela do rerun.io a mostrar a imagem da câmara em tempo real, apresentando visualmente o efeito.](../../en/images/d22-02.png)

## Se encontrar o seguinte erro

A câmara não consegue ligar-se, mas a troca de câmaras no Tencent Meeting ainda abre normalmente

![A imagem mostra a interface da linha de comandos do Windows, com os resultados de deteção de câmaras. Na parte superior aparece o texto "Administrador: Prompt de Comando - C:\Users\40743\miniconda3\condabin\conda.bat activate leroobot". Abaixo apresenta a informação "Detected Cameras", listando "Camera #0" como OpenCV Camera @ 0, com Type OpenCV, Backend api DSHOW, Default stream profile e outros parâmetros, e mostra ainda mensagens de erro como "ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)". Esta imagem está relacionada com o contexto do problema de ligação da câmara descrito no documento, apresentando visualmente a informação de erro relacionada.](../../en/images/d22-03.png)

Modifique o ficheiro `lerobot\src\lerobot\cameras\utils.py` para alterar o backend do OpenCV para `cv2.CAP_SHOW`

![Esta imagem mostra um trecho de código Python, parte do conteúdo usado para modificar o ficheiro `utils.py` da biblioteca lerobot, correspondendo ao passo de resolução do erro de ligação da câmara no Windows descrito no documento. O código define a função `get_cv2_backend()`, que define o backend do OpenCV para diferentes sistemas: quando o sistema é Windows, o `return int(cv2.CAP_MSMF)` originalmente comentado fica comentado e o conteúdo atualmente realçado é `return int(cv2.CAP_DSHOW)`, com uma nota ao lado a explicar que esta abordagem substitui o AVFOUNDATION; a função também define o backend OpenCV correspondente para os sistemas macOS e Linux. É a modificação de código central para resolver o problema de ligação da câmara indicado.](../../en/images/d22-04.png)

> Este é um erro que nem o Doubao consegue resolver; é tudo porque a biblioteca lerobot está demasiado encapsulada e é muito difícil para os principiantes depurarem

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## Ligar várias câmaras, teleoperação com as imagens das câmaras apresentadas

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
