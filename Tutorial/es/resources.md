[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | Español | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>Algunos recursos</title>

# El [archivo URDF](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf) oficial de Lerbot



## URDF Studio

https://urdf.d-robotics.cc/



## Control de simulación ROS2 (impleméntalo tú mismo)

https://github.com/holmsslk/so-arm-moveit-hardware



## Interfaz gráfica oficial de LeRobot

https://github.com/huggingface/leLab

LeLab es una aplicación web que reúne todo el flujo de trabajo de LeRobot —calibración, teleoperación, grabación, entrenamiento, reproducción— en una única interfaz de navegador. Solo tienes que conectar el brazo robótico, abrir la aplicación y ponerte a trabajar. Sin engorrosos comandos de línea de comandos ni entrada de teclado.

🤗 El punto de entrada web nativo de LeRobot, diseñado para que los nuevos usuarios pasen de "recién sacado de la caja" a "entrenar su primera política" en cuestión de minutos.

🤗 Instala y ejecuta todo con un solo comando.



# Controlar el brazo Follower desde un teléfono

https://huggingface.co/docs/lerobot/main/en/phone_teleop





## Desarrollo de robótica en la nube: dispositivos ROS 2 y simulación LeRobot con Isaac Sim y transmisión de datos en AWS

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## Configurar los IDs de los servos y la calibración del centro en la interfaz web

https://bambot.org/feetech.js?lang=zh

1. Introduce 0 o 1 según el modelo del servo y, a continuación, haz clic en "Connect"

![La imagen muestra la interfaz de conexión al configurar los IDs de los servos y la calibración del centro en la interfaz web. En la interfaz hay una sección "Connect" que incluye un cuadro de selección de velocidad en baudios (1,000,000 bps, Index 0), un cuadro de selección de extremo de protocolo (0=STS/SMS, 1=SCS, actualmente seleccionado 0), un botón de conexión (verde, marcado con el número 2) y una visualización de estado (roja, que muestra "Desconectado"). Esta imagen se corresponde con el texto anterior "Introduce 0 o 1 según el modelo del servo y haz clic en 'Connect'" y presenta visualmente la configuración de la interfaz de la operación de conexión.](../en/images/d58-01.png)

2. Escanea los servos con IDs 1\~6; usa FOUND en los resultados del escaneo para confirmar el servo con el ID correspondiente. Por ejemplo, en la imagen se encontró el servo con ID 1

![Esta es una captura de la interfaz de escaneo de servos del software relacionado con Lerbot; su contenido central es el proceso y el resultado de la operación de escaneo de servos. La interfaz tiene campos de entrada "ID inicial" e "ID final", con 1 y 6 introducidos respectivamente, además de un botón "Iniciar escaneo", y debajo la zona de visualización de resultados de escaneo correspondiente, que se corresponde con el contenido sobre el ID del servo escaneado mencionado en el documento. El registro de resultados del escaneo muestra que solo al escanear el ID1 se obtuvo correctamente la información del servo, mostrando "FOUND: Pos-1230, Mode=0, BaudIdx=0", mientras que al escanear del ID2 al ID6 apareció en cada caso un aviso de error de lectura anómala de la posición y ausencia de paquete de estado.](../en/images/d58-02.png)

3. Configuración de ID y calibración del centro

① Configura la entrada del ID del servo actual con el ID del servo escaneado

② Introduce un número en "ID management" y haz clic en "Change ID" para configurar el ID

③ Calibración del centro (el centro del servo STS3215 es 2047, el centro del servo SCS0009 es 511)

Servo STS: introduce 2047 en "Position control" y haz clic en "Set"

Servo SCS: introduce 511 en "Position control" y haz clic en "Set"

![La imagen muestra la interfaz de control de un solo servo del URDF Studio oficial de Lerbot. En la interfaz, el "Current servo ID" se muestra como 1; debajo, en "ID management", se puede introducir un número y hacer clic en "Change ID"; en el área "Position Control" hay un botón "Read position" que muestra la posición 2047, junto a un botón "Set". Esta imagen está muy relacionada con el contexto y presenta visualmente los pasos mencionados en él: introducir como ID del servo actual el ID del servo escaneado, introducir un número en "ID management" y hacer clic en "Change ID" para configurar el ID, e introducir 2047 en "Position control" y hacer clic en "Set" para el servo STS.](../en/images/d58-03.png)
