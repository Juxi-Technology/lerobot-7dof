[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | Español | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | [한국어](../ko/06-teleoperation-with-camera/windows.md) | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Computadora con Windows

## Conectar la cámara a la computadora

```Shell
lerobot-find-cameras opencv
```

![Esta imagen es la interfaz de error de la línea de comandos de Windows y corresponde al escenario de error "la cámara no se puede conectar" mencionado en el documento. La información de error que muestra la interfaz consta de dos partes: por un lado, el error global "Camera index out of range" y, por otro, el error del script relacionado con lerobot "Failed to connect or configure OpenCV camera 0". La interfaz también enumera las cámaras detectadas: en total 2 dispositivos llamados OpenCV Camera, ambos con backend DSHOW, formato YUY2, ancho 640, alto 480 y velocidad de fotogramas 0.0 FPS. Este contenido de error se corresponde con la situación descrita en el documento: la cámara no se puede conectar, pero sí funciona con normalidad al usarla en Tencent Meeting.](../../en/images/d22-01.png)

## Teleoperación con la imagen de la cámara mostrada

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

Se abre la ventana de rerun.io, que muestra en tiempo real la trayectoria de cada articulación del servo, junto con la imagen en vivo de la cámara

y guarda las imágenes en el directorio `C:\Users\username\outputs\captured_images`

![La imagen muestra la ventana de rerun.io, que presenta en tiempo real las trayectorias de cada articulación del servo y la imagen en vivo de la cámara. A la izquierda está la interfaz de blueprint, con opciones como "teleoperation". En el centro está el gráfico de trayectorias, que muestra datos de posición de articulaciones como "observation_wrist_rot.pos". A la derecha está la imagen de la cámara, que muestra la escena desde el punto de vista del robot. En la esquina superior derecha se muestra "Waiting for data on rerun: http://127.0.0.1:9876/remote..." y debajo hay información del origen de datos. Esta imagen se relaciona con el contenido que describe la ventana de rerun.io mostrando la imagen de la cámara en tiempo real y presenta visualmente el efecto de la pantalla.](../../en/images/d22-02.png)

## Si te encuentras con el siguiente error

La cámara no se puede conectar, pero al cambiar de cámara en Tencent Meeting sí se abre normalmente

![La imagen muestra la interfaz de línea de comandos de un ordenador con Windows, con los resultados de detección de cámaras. En la parte superior aparece el texto "Administrador: Símbolo del sistema - C:\Users\40743\miniconda3\condabin\conda.bat activate leroobot". Debajo se muestra la información "Detected Cameras", que lista "Camera #0" como OpenCV Camera @ 0, con Type OpenCV, Backend api DSHOW, Default stream profile y otros parámetros, además de avisos de error como "ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)". Esta imagen se relaciona con el contexto del problema de conexión de la cámara descrito en el documento y presenta visualmente la información de error correspondiente.](../../en/images/d22-03.png)

Modifica el archivo `lerobot\src\lerobot\cameras\utils.py` para cambiar el backend de OpenCV a `cv2.CAP_SHOW`

![Esta imagen muestra un fragmento de código Python que forma parte del archivo `utils.py` de la biblioteca lerobot y que corresponde al paso de solución del error de conexión de cámara en un ordenador con Windows descrito en el documento. El código define la función `get_cv2_backend()`, que establece el backend de OpenCV según el sistema: cuando el sistema es Windows, el `return int(cv2.CAP_MSMF)` original aparece comentado y el contenido resaltado actualmente es `return int(cv2.CAP_DSHOW)`, con una nota al lado que explica que se usa esta opción en lugar de AVFOUNDATION; la función también define el backend de OpenCV correspondiente para macOS y Linux. Es el contenido central de la modificación de código que resuelve el problema de conexión de la cámara indicada.](../../en/images/d22-04.png)

> Este es un error que ni Doubao puede resolver; se debe a que la biblioteca lerobot está encapsulada en exceso y a los principiantes les cuesta mucho depurarlo

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## Conectar varias cámaras, teleoperación con las imágenes de las cámaras mostradas

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
