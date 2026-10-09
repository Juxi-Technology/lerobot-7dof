[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | Español | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# Errores comunes y soluciones

## Falla la captura de la cámara

![La imagen muestra la información de salida del terminal al ejecutar el código relacionado con el robot LeroBot. El registro `INFO` muestra la apertura de la cámara de OpenCV y la desconexión del Follower; el registro `ERROR` señala que, en el archivo `camera_opencv.py`, la función `read` lanzó un `RuntimeError` por `OpenCVCamera(0) read failed`. Esta imagen se relaciona con el problema "Falla la captura de la cámara" del documento y presenta visualmente el error que aparece al ejecutar el código, ayudando a comprender cómo se manifiesta este problema en la práctica.](../../en/images/d55-01.png)

Comprueba si el cable de la cámara de muñeca está flojo, sobre todo el extremo junto a la cámara: ese conector es muy propenso a hacer mal contacto

## La cámara se desconecta

![La imagen muestra la interfaz con el código y la información de error del problema de fallo en la captura de la cámara que aparece durante la ejecución de LeroBot HF. En la parte superior se muestra el aviso de fallo de conexión de la cámara de OpenCV; debajo está la pila de errores de la función `record_loop` del archivo `lerobot_record.py`, que indica que se produjo un `TimeoutError` en `async_read` y avisa de que la cámara no obtuvo un fotograma en 200 ms. Esta imagen corresponde al contexto del problema "Falla la captura de la cámara" del documento y presenta visualmente cómo se manifiesta el problema a nivel de código.](../../en/images/d55-02.png)

Reinicia la línea de comandos

## Problema de comunicación con el servo 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![Esta imagen es un registro de error del terminal y corresponde al contenido relacionado con el problema de comunicación con el servo 1 mencionado en el documento. El registro muestra la ruta concreta del archivo de código; en las líneas 1069 y 1181 del archivo `lerobot/src/lerobot/motors/motors_bus.py` se registra el mensaje de error `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`, y también se indica la causa del error `There is no status packet!`, presentando de forma completa los detalles del error del problema de comunicación con el servo 1 mencionado en el documento.](../../en/images/fix-04.png)

La solución: cambia todos los `num_retry` del código en `lerobot/src/lerobot/motors/motors_bus.py` a 99, especialmente el de la línea que da el error

![La imagen muestra el contenido del archivo de código `motors_bus.py` del proyecto LeroBot. En el archivo aparece resaltado el método `write` de la clase `MotorsBusABC`, en el que la variable `num_retry` se ha cambiado a `99`. Esta imagen se relaciona con la sección "Problema de comunicación con el servo 1" del documento y corresponde a la solución mencionada de cambiar todos los `num_retry` del código en `lerobot/src/lerobot/motors/motors_bus.py` a 99, especialmente la parte del código de la línea que da el error.](../../en/images/d55-03.png)

## Problema de comunicación con el servo 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![Esta imagen muestra un registro de error que forma parte del contenido del error correspondiente al "Problema de comunicación con el servo 2" del documento. En concreto, al ejecutar el código aparece un error llamado ConnectionError, cuya manifestación es Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries; la información de error adjunta también indica There is no status packet, y además se marca la ruta del código que provocó el error. Este registro de error se corresponde por completo con la descripción del "Problema de comunicación con el servo 2" del documento y es el registro real de dicho error.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![La imagen muestra la interfaz de operación de la línea de comandos con el terminal zsh en el sistema macOS. El terminal muestra varias rutas de archivos y contenido de archivos de código; en el archivo `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` aparece el mensaje de error `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`. Esta imagen se relaciona con el contenido "Problema de comunicación con el servo 2" del documento y presenta visualmente la situación de la interfaz del terminal en el momento del error.](../../en/images/fix-05.png)
</column>
</grid>

Solución: recalibra el brazo robótico
