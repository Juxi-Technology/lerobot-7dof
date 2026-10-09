[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | Español | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Computadora con Mac

## Consultar el puerto

```Shell
ls /dev/tty.*
```

El resultado es similar a la imagen siguiente; cualquiera de los dos puertos sirve

![La imagen muestra la salida en el terminal de Mac tras ejecutar el comando "ls /dev/tty.*", que enumera varios puertos. Dos de ellos, "/dev/tty.usbmodem5AAF2194741" y "/dev/tty.wchusbserial5AAF2194741", están resaltados con recuadros rojos. El contexto menciona la consulta de puertos, y el resultado es similar a esta imagen; se puede usar cualquiera de los dos puertos. La imagen presenta visualmente la información de puertos que hay que consultar, está estrechamente relacionada con el contenido anterior "Consultar el puerto" y es una muestra del resultado de la operación de consulta.](../../en/images/d09-01.png)

## Otorgar permisos al puerto

Dar a todos los usuarios permiso para leer y escribir estos dispositivos serie

```Shell
chmod 666 /dev/tty.*
```

## Registrar mis puertos

Brazo Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Brazo Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## ¿Por qué hay dos puertos en un Mac?

La placa controladora de servos que usamos **es reconocida por macOS como dos tipos diferentes de controladores serie a la vez**, por lo que se muestran dos puertos:

- Uno es el controlador serie genérico predeterminado del sistema (`/dev/tty.usbmodemxxxx`)
- El otro es el controlador serie dedicado que proporciona el fabricante del chip (por ejemplo, aquí "wch" se refiere al chip CH340/CH341 de Nanjing Qinheng) (`/dev/tty.wchusbserialxxxx`)

Esto es normal: **los dos puertos en realidad corresponden al mismo dispositivo de hardware**, y puedes conectarte y comunicarte a través de cualquiera de ellos (por ejemplo, basta con elegir cualquiera de los dos puertos en el software que controla el brazo robótico).

Si más adelante te topas con un error al usar un puerto, prueba a cambiar al otro.
