[English](../en/01-install-lerobot/ubuntu.md) | [简体中文](../zh-hans/01-install-lerobot/ubuntu.md) | [繁體中文](../zh-hant/01-install-lerobot/ubuntu.md) | [Deutsch](../de/01-install-lerobot/ubuntu.md) | Español | [Français](../fr/01-install-lerobot/ubuntu.md) | [Italiano](../it/01-install-lerobot/ubuntu.md) | [日本語](../ja/01-install-lerobot/ubuntu.md) | [한국어](../ko/01-install-lerobot/ubuntu.md) | [Português (BR)](../pt-br/01-install-lerobot/ubuntu.md) | [Português (PT)](../pt-pt/01-install-lerobot/ubuntu.md)

# Computadora con Ubuntu

El brazo Leader negro utiliza un adaptador de corriente de 5V6A.

El brazo Follower blanco utiliza un adaptador de corriente de 12V5A.

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
conda create -y -n lerobot python=3.12 -y
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

<grid>
<column width-ratio="0.568354">
![La imagen muestra el resultado de activar un entorno virtual de conda e instalar ffmpeg en una computadora con Ubuntu. Primero, conda activa el entorno virtual llamado lerobot; después se ejecuta el comando conda install ffmpeg=7.1.1 -c conda -forge, que muestra la información de los Channels de conda, incluido conda - forge; y por último aparece la Platform linux - 64 junto con las operaciones Collecting package metadata y Solving environment ya finalizadas. Esta imagen corresponde a la sección "Instalar ffmpeg" y presenta visualmente la ejecución de la instalación.](../../en/images/d03-01.png)
</column>
<column width-ratio="0.431646">
![Esta imagen es una captura de pantalla del terminal de Ubuntu que muestra el resultado devuelto tras ejecutar el comando ffmpeg. Se aprecia la versión de ffmpeg 7.1.1, su información de configuración y los números de versión de los módulos compatibles (como libavcodec y libavformat), junto con las notas de uso del conversor multimedia universal. Corresponde al paso de verificación posterior a instalar ffmpeg y sirve para confirmar que la herramienta ffmpeg se instaló correctamente en el sistema.](../../en/images/d03-02.png)
</column>
</grid>

## Descargar el repositorio oficial de LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar el repositorio de código

```Shell
#cd lerobot-main
cd lerobot

pip install -e ".[feetech]"
```

<grid>
<column width-ratio="0.562345">
![Esta imagen muestra el proceso de entrar al directorio lerobot en el terminal de Ubuntu y ejecutar el comando `pip install -e.\[feetech\]`, que forma parte de la instalación del repositorio oficial de LeRobot. Muestra con claridad cada paso de la ejecución del comando, incluida la obtención de paquetes desde el repositorio de Huawei Cloud especificado, la instalación de dependencias y la descarga de paquetes de conjuntos de datos relacionados (como diffusers, huggingface-hub y accelerate), donde varios paquetes aparecen marcados con su progreso de descarga, tamaño y velocidad, y termina con un mensaje de que las dependencias ya están satisfechas, completando así la instalación del repositorio.](../../en/images/fix-02.png)
</column>
<column width-ratio="0.437655">
![Esta imagen muestra la interfaz de línea de comandos del terminal de Ubuntu durante la instalación de paquetes de software, incluida la información de manejo de dependencias al instalar programas como ffmpeg. El terminal muestra la lista de paquetes que se están procesando, como pytz, pyyaml y numpy, así como el proceso de desinstalar las versiones existentes e instalar las nuevas, con notas sobre la coherencia de las dependencias. Este contenido corresponde al paso "Verificar la instalación" posterior a "Instalar ffmpeg" y es un registro de la salida del terminal al verificar el proceso de instalación de ffmpeg y otro software.](../../en/images/d03-03.png)
</column>
</grid>

## Verificar la instalación

```Shell
lerobot-info

python

import lerobot
lerobot.__version__

import torch
torch.cuda.is_available()
import scservo_sdk
```

## Resultados en un equipo con 4090

![La imagen muestra los comandos y la información de LeRobot ejecutándose en una computadora con Ubuntu. El comando "Lerobot lerobot -info" muestra la versión 0.4.3 de LeRobot, la plataforma Linux - 5.15.0 - 105 - generic - x86_64 - with - glibc2.35, la versión de Python 3.12.0 y otra información. En ella, la versión de PyTorch es 2.7.1 + cu126, la versión de CUDA es 12.6 y el modelo de GPU es NVIDIA GeForce RTX 4090. Esta imagen se relaciona con la verificación de una instalación correcta, ya que muestra la información de funcionamiento de LeRobot en el entorno Ubuntu.](../../en/images/d03-04.png)

![La imagen muestra la interfaz interactiva de Python mientras se ejecuta el repositorio LeRobot en una computadora con Ubuntu. Muestra la versión de Python 3.10.12, incluida la información del empaquetado de conda-forge y la fecha de compilación. El usuario introdujo sucesivamente los comandos `import lerobot`, `lerobot.__version__`, `import torch`, `torch.cuda.is_available()` e `import scservo_sdk`, y obtuvo el número de versión 0.4.3 de LeRobot, la disponibilidad de CUDA como True y la importación correcta de `scservo_sdk`. Esta imagen se relaciona con la verificación de la instalación del repositorio LeRobot y presenta visualmente dicho proceso.](../../en/images/d03-05.png)

## Resultados en un NVIDIA DGX Spark

![La imagen muestra la salida del terminal al ejecutar el repositorio LeRobot en una computadora con Ubuntu. Muestra la versión 0.4.4 de LeRobot, la versión de CUDA 13.0 y el modelo de GPU NVIDIA GeForce GTX 1660 Ti. También enumera versiones de bibliotecas como HuggingFace Hub, Datasets y PyTorch, así como las versiones de las herramientas FFmpeg y PyTorch. Por último, verifica las importaciones de LeRobot y torch; torch.cuda.is_available() devuelve True, lo que indica que CUDA está disponible. Esta imagen se relaciona con la ejecución del repositorio LeRobot en una computadora con Ubuntu y muestra sus resultados.](../../en/images/d03-06.png)
