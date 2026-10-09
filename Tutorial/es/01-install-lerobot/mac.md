[English](../en/01-install-lerobot/mac.md) | [简体中文](../zh-hans/01-install-lerobot/mac.md) | [繁體中文](../zh-hant/01-install-lerobot/mac.md) | [Deutsch](../de/01-install-lerobot/mac.md) | Español | [Français](../fr/01-install-lerobot/mac.md) | [Italiano](../it/01-install-lerobot/mac.md) | [日本語](../ja/01-install-lerobot/mac.md) | [한국어](../ko/01-install-lerobot/mac.md) | [Português (BR)](../pt-br/01-install-lerobot/mac.md) | [Português (PT)](../pt-pt/01-install-lerobot/mac.md)

# Computadora con Mac

El brazo Leader negro utiliza un adaptador de corriente de 5V6A.

El brazo Follower blanco utiliza un adaptador de corriente de 12V5A.

## Otorgar permisos

![Esta imagen muestra una ventana de configuración del sistema Mac, actualmente en la página de accesibilidad, con Privacidad y seguridad seleccionado en la barra lateral izquierda. La ventana enumera varias aplicaciones, entre ellas Baidu Netdisk, DingTalk y Doubao; el interruptor de la app Terminal está resaltado con un recuadro rojo y está activado. Esto corresponde al paso "Otorgar permisos" del flujo de Mac, que habilita los permisos pertinentes del Terminal como preparación para instalar Miniconda y cambiar los espejos más adelante.](../../en/images/d04-01.png)

## Instalar Miniconda

https://www.anaconda.com/download

## Cambiar el espejo de pip

```Shell
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple
```

## Cambiar el espejo de conda

```Shell
# Borrar la configuración existente de .condarc (opcional, para evitar conflictos)
echo "" > ~/.condarc

# Escribir la configuración del espejo de Tsinghua
cat << EOF > ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
EOF

# Limpiar la caché para que la configuración surta efecto
conda clean -i
```

## Crear un entorno virtual

```Shell
conda create -y -n lerobot python=3.12
```

## Activar el entorno virtual

```Shell
conda activate lerobot
```

## Instalar ffmpeg

```Shell
conda install ffmpeg=7.1.1 -c conda-forge -y
```

Verificar que la instalación se haya realizado correctamente

```Shell
ffmpeg
```

![Esta imagen muestra la salida del terminal después de ejecutar el comando `ffmpeg`, utilizada para verificar que ffmpeg se instaló correctamente y correspondiente al paso de verificación posterior a "Instalar ffmpeg". La salida muestra con claridad la versión FFmpeg 7.1.1, los derechos de autor de los desarrolladores de FFmpeg de 2000 a 2025, junto con los indicadores de compilación, la lista de codificadores compatibles y las notas de uso del conversor multimedia universal, y termina con una sugerencia de usar la opción `-h` o el comando `man ffmpeg` para obtener más ayuda.](../../en/images/d04-02.png)

## Descargar LeRobot

- Descargar el repositorio oficial de LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar el repositorio de código

```Shell
# cd lerobot-main
cd lerobot
pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.500000">
![La imagen muestra la línea de comandos instalando el paquete "feetech" con pip en un Mac. Muestra el progreso de la instalación, incluida la obtención de paquetes desde "https://repo.huaweicloud.com/repository/pypi/simple/" y la descarga de varios archivos como datasets, diffusers y huggingface-hub, y termina con la descarga de "einops==0.8.0". Esta imagen se relaciona con la sección "Instalar el repositorio de código" y presenta visualmente la ejecución del comando y su resultado al instalar el repositorio.](../../en/images/d04-03.png)
</column>
<column width-ratio="0.500000">
![La imagen muestra la pantalla de verificación en un Mac después de instalar el repositorio de código LeRobot. El terminal muestra "Successfully installed LeRobot" y enumera varios paquetes de Python instalados con sus versiones, como numpy y pandas. Esta imagen corresponde a las secciones "Instalar el repositorio de código" y "Verificar la instalación" y presenta visualmente los paquetes instalados para que el usuario pueda confirmar que la instalación se realizó correctamente.](../../en/images/d04-04.png)
</column>
</grid>

## Verificar la instalación

```Shell
lerobot-info

python

import lerobot
import torch
torch.cuda.is_available()
import scservo_sdk
```

![Esta imagen es una captura de pantalla de la línea de comandos del terminal de Mac, parte de la verificación de la configuración en los pasos de instalación de LeRobot. Muestra el directorio actual del proyecto, lerobot-main; tras el comando python entra en el entorno interactivo de Python con la versión 3.10.19 sobre el sistema darwin. Se ejecutaron sucesivamente los comandos import lerobot, import torch y torch.cuda.is_available(), y el resultado mostró la disponibilidad de CUDA como False, seguido de import scservo_sdk. Esto corresponde al paso de verificación, utilizado para confirmar el estado de instalación y configuración de LeRobot y sus dependencias.](../../en/images/d04-05.png)

![La imagen muestra la salida del terminal de un comando de LeRobot en un Mac. Muestra la versión 0.4.3 de LeRobot, la plataforma macOS - 15.6.1 - arm64 - arm - 64bit, la versión de Python 3.12.12 y otra información. También enumera la información de versión de Huggingface Hub, Datasets, NumPy, FFmpeg y PyTorch, si PyTorch está compilado con soporte para CUDA, la versión de CUDA y el modelo de GPU, y por último la lista de scripts de LeRobot. Esta imagen corresponde al contexto de verificación y muestra la información de LeRobot tras la instalación.](../../en/images/d04-06.png)
