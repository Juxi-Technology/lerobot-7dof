[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | Español | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Computadora con Windows



<callout emoji="🚫">
Los brazos Leader y Follower deben estar conectados
</callout>

## Calibrar el brazo Follower

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![La imagen muestra la interfaz de línea de comandos en una computadora con Windows al ejecutar una calibración de lerobot. El comando es "lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm". La interfaz muestra información de calibración, incluidos mensajes como "zihao_follower_arm SO10IFollower connected", y también enumera los valores NAME, MIN, POS y MAX de cada articulación del brazo robótico. Durante la calibración, se pide al usuario que mueva el brazo robótico al centro de su rango de movimiento y pulse ENTER, mientras se registran las posiciones, y que pulse ENTER para detener. Esta imagen se relaciona con la calibración del brazo Follower y muestra los pasos concretos y la respuesta de la interfaz.](../../en/images/d14-01.png)

## Calibrar el brazo Leader

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![La imagen muestra la interfaz de línea de comandos calibrando el brazo robótico con el comando lerobot-calibrate en una computadora con Windows. Muestra información sobre la calibración de los brazos Follower y Leader, incluida la ruta de guardado de la posición de calibración, el tipo de robot, el número de puerto y el ID. También te pide que muevas el Follower al centro de su rango de movimiento y pulses ENTER, que recorras cada articulación por todo su rango de movimiento, que registres las posiciones y que pulses ENTER para detener. En la parte inferior muestra el nombre, el valor mínimo, la posición actual y el valor máximo de cada articulación.](../../en/images/d14-02.png)

## Dónde se exportan los archivos

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## Calibrar otro brazo robótico

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## Notas

### ① Un brazo deja de moverse al alcanzar un límite

Hay que recalibrarlo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② No se encuentran los servos

![La imagen muestra un mensaje de error al ejecutar el programa de lerobot en macOS. Mientras el programa se ejecuta, aparece un RuntimeError: "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'", que indica IDs de servo ausentes, incluidos los servos del 1 al 6, todos con un número de modelo esperado de 777, pero la lista de servos realmente encontrados está vacía. Esto se relaciona con la nota "no se encuentran los servos"; puede deberse a que los servos no están conectados, así que vuelve a conectarlos y gira el conector.](../../en/images/d14-03.png)

La alimentación de los servos no está conectada; vuelve a conectarla y gira el conector
