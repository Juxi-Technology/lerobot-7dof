[English](../en/01-install-lerobot/windows.md) | [简体中文](../zh-hans/01-install-lerobot/windows.md) | [繁體中文](../zh-hant/01-install-lerobot/windows.md) | [Deutsch](../de/01-install-lerobot/windows.md) | Español | [Français](../fr/01-install-lerobot/windows.md) | [Italiano](../it/01-install-lerobot/windows.md) | [日本語](../ja/01-install-lerobot/windows.md) | [한국어](../ko/01-install-lerobot/windows.md) | [Português (BR)](../pt-br/01-install-lerobot/windows.md) | [Português (PT)](../pt-pt/01-install-lerobot/windows.md)

# Computadora con Windows

El brazo Leader negro utiliza un adaptador de corriente de 5V6A.

El brazo Follower blanco utiliza un adaptador de corriente de 12V5A.

## Instalar Miniconda

anaconda.com/download/success

O haz clic en este enlace para descargar el instalador directamente

https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe

<grid>
<column width-ratio="0.500000">
![Esta imagen es la pantalla de instalación de Miniconda3 en Windows, que muestra la versión del software py312_24.7.1-0 (64-bit). La pantalla ofrece dos opciones de tipo de instalación, en las que la opción "Just Me (recommended)" está resaltada con un recuadro rojo y es el método recomendado seleccionado actualmente, mientras que la otra opción, "All Users (requires admin privileges)", no está seleccionada. En la parte superior de la pantalla se te pide que elijas un tipo de instalación para Miniconda3, y en la parte inferior hay tres botones: "Back", "Next" y "Cancel". Esta pantalla es el paso clave del flujo de instalación de Miniconda para confirmar el alcance de la instalación.](../../en/images/d05-01.png)
</column>
<column width-ratio="0.500000">
![La imagen muestra las opciones avanzadas de instalación en la pantalla de instalación de Miniconda3. La opción "Add Miniconda3 to my PATH environment variable" está resaltada con un recuadro rojo, con una nota al lado que explica que no se recomienda porque puede entrar en conflicto con otras aplicaciones, y sugiere los menús del Símbolo del sistema y de PowerShell añadidos al menú Inicio de Windows. Esta imagen se relaciona con el paso de crear un entorno virtual posterior a "Cambiar el espejo de conda", y es una referencia de configuración al instalar Miniconda.](../../en/images/d05-02.png)
</column>
</grid>

## Cambiar el espejo de conda

```Shell
# Primero borrar la configuración de espejos existente (para evitar conflictos)
conda config --remove-key channels

# Reemplazar los canales predeterminados de conda y los canales de terceros habituales por el espejo de Tsinghua
# Añadir los canales de paquetes predeterminados (main/r/msys2)
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2/

# Añadir los canales de terceros habituales
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

# Activar la visualización del origen de descarga, para que se muestre la dirección exacta al instalar paquetes
conda config --set show_channel_urls yes

# Limpiar la caché de índices para que los nuevos espejos surtan efecto
conda clean -i

# Ver la configuración actual (para verificar que los canales se añadieron correctamente)
conda config --show-sources
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

![Esta imagen es la ventana de la línea de comandos de Windows y muestra el resultado de la verificación tras ejecutar el comando ffmpeg. En concreto, la línea de comandos muestra la versión de ffmpeg 7.1.1, con la información de derechos de autor y de compilación, junto con información sobre los archivos de biblioteca asociados a ffmpeg, y notas de uso en la parte inferior que cubren el uso básico y cómo obtener más ayuda. Esta imagen sirve para verificar que ffmpeg se instaló correctamente en una computadora con Windows, corresponde al paso de verificación posterior a "Instalar ffmpeg" y presenta visualmente el estado de funcionamiento una vez completada la instalación de ffmpeg.](../../en/images/d05-03.png)

<grid>
<column width-ratio="0.568354">
![Esta es la interfaz del terminal de Linux, que muestra comandos relacionados con conda y su ejecución. Se marcan claramente dos comandos principales: el comando para activar el entorno virtual llamado lerobot, "$ conda activate lerobot", y el comando para desactivar el entorno activo, "$ conda deactivate". El entorno (base) está activado actualmente y el terminal está ejecutando la instalación de ffmpeg 7.1.1 desde el canal conda-forge, mostrando varias direcciones de espejo configuradas, mientras que el flujo de recopilación de metadatos de paquetes y del entorno de dependencias ya está completo.](../../en/images/d05-04.png)
</column>
<column width-ratio="0.431646">
![La imagen muestra la interfaz del terminal usando el comando ffmpeg en Ubuntu. Muestra la información de versión de ffmpeg, incluidos el número de versión y los detalles de configuración del compilador y del responsable de la compilación. También enumera las versiones de varios códecs, como libavcodec y libavformat. Las notas de uso están en la parte inferior, donde se sugiere usar "-h" para toda la ayuda o ejecutar "man ffmpeg". Esta imagen se relaciona con la sección "Instalar ffmpeg" y sirve para verificar una instalación correcta de ffmpeg mostrando su versión e información de compilación.](../../en/images/d05-05.png)
</column>
</grid>

## Descargar LeRobot

- Descargar el repositorio oficial de LeRobot

```Shell
git clone https://github.com/huggingface/lerobot.git
```

## Instalar el repositorio de código

```Shell
cd lerobot
```

```Plain Text
pip install -e ".[feetech]"
```

![La imagen muestra el resultado de la verificación en la línea de comandos cmd de Windows después de instalar el repositorio de código LeRobot. La línea de comandos muestra mensajes como "Successfully built lerobot", lo que indica que la instalación se realizó correctamente. También enumera varios paquetes de Python y sus números de versión, como numpy 1.22.3 y scipy 1.7.1. En la parte inferior se ve el indicador "(lerobot) C:\\Users\\40743\\Downloads\\lerobot>", que señala que el directorio actual es la carpeta lerobot dentro de Downloads. Esta imagen corresponde a la sección "Verificar la instalación" y presenta visualmente la respuesta de la línea de comandos tras una instalación correcta.](../../en/images/d05-06.png)

## Verificar la instalación

```Shell
python

import lerobot
import scservo_sdk
import torch
torch.cuda.is_available()
```

![La imagen muestra la pantalla que verifica una instalación correcta en el entorno de Python en Windows. La línea de comandos muestra la versión de Python 3.10.19, ejecutó código importando módulos como lerobot, scservo_sdk y torch, y por último ejecutó torch.cuda.is_available(), que devolvió False. Esta imagen corresponde a la sección "Verificar la instalación" y presenta visualmente la operación y el resultado de verificar una instalación correcta a través del entorno de Python después de instalar el repositorio de código LeRobot.](../../en/images/d05-07.png)
