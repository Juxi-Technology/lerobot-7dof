[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | Español | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [한국어](../ko/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>Subir el conjunto de datos a HuggingFace (opcional)</title>

# Método 1: subir localmente (no recomendado; velocidad de subida lenta)

- Subida automática

Establece `push_to_hub=true` al recopilar el conjunto de datos y se subirá automáticamente al finalizar la recopilación

![Esta imagen muestra una interfaz de línea de comandos, parte del registro de ejecución del proceso de subida del conjunto de datos. En la parte superior muestra información del entorno de herramientas como SVN y treet W2; en el centro hay un mensaje de procesamiento como "Starting the second pass: moving the mov atom to the beginning of the file", y debajo hay errores de la ejecución como "error messaging the mach port for IMCRunLoopWakeUpReliable", mientras que a la derecha enumera el progreso de procesamiento y las cifras de transferencia de datos, como la cantidad de datos y la velocidad de distintas entradas. En conjunto, presenta un registro del estado de ejecución durante el procesamiento de la subida del conjunto de datos.](../../en/images/d28-01.png)

- Subida manual

Establece `push_to_hub=false` al recopilar el conjunto de datos y súbelo manualmente al finalizar la recopilación

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



Ya sea automática o manual, la velocidad de subida es muy lenta (unos 100 KB por segundo)

porque los servidores de HuggingFace están en el extranjero

# Método 2: subir desde una plataforma de GPU en la nube (recomendado)

## Iniciar sesión en la plataforma de GPU en la nube Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## Iniciar una instancia de GPU en la nube

## Subir el archivo comprimido del conjunto de datos a `Datasets`

## Copiar el comando de descarga de la instancia

![La imagen muestra la página de datasets de la plataforma Featurize. En la parte superior se ve el título "Datasets" y debajo hay un conjunto de datos llamado "soarm_amazing_hand_pick.zip", de 213.3 MB, subido hace 16 horas. A la derecha hay un botón "Cloud Unzip", junto con botones como "Like", "Comment" y "Copy Instance Download Command". Esta imagen se relaciona con el paso "Subir el archivo comprimido del conjunto de datos a `Datasets`" y muestra la página de datasets tras la subida.](../../en/images/d28-02.png)

## Ejecutar en la línea de comandos de la instancia de GPU en la nube

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## Subir el conjunto de datos a HuggingFace

Crea un archivo `upload_dataset.py` con el siguiente contenido

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

Ejecuta el archivo

```Shell
python upload_dataset.py
```

![Esta imagen muestra el proceso de ejecutar la subida del conjunto de datos en la línea de comandos de una instancia de GPU en la nube, donde un usuario llamado lerobot2 ejecutó el comando python upload.py. Muestra el progreso de procesamiento de los archivos, con 6 archivos que procesar y todos al 100% de progreso, y marca el tamaño de transferencia de cada archivo, con un progreso total de transferencia de datos del 100%. En la parte inferior indica que no se ha modificado ningún archivo desde el último commit, por lo que se omite el commit para evitar crear un commit vacío. Este contenido corresponde al paso de ejecutar el archivo upload_dataset.py.](../../en/images/d28-03.png)

- Otro método de subida (no recomendado)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![Esta imagen muestra el proceso de subir un conjunto de datos con el comando `hf upload` de HuggingFace, con el comando `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`. La imagen muestra que la subida ha entrado en su fase final, con todos los archivos al 100% de progreso, incluidos varios archivos de vídeo y archivos parquet cuyos tamaños de subida coinciden exactamente con los tamaños de los archivos locales correspondientes, junto con el tamaño total de archivos subidos y la velocidad de transferencia, y en la parte inferior un enlace a la página del conjunto de datos de HuggingFace de este commit de subida, lo que indica que la tarea de subida se ha completado.](../../en/images/d28-04.png)

# Ver el conjunto de datos en HuggingFace

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![Esta imagen es una captura de pantalla de la página de detalles del conjunto de datos soarm_amazing_hand_pick del equipo Juxi-Technology en la plataforma Hugging Face y corresponde al contenido "Ver el conjunto de datos en HuggingFace". En la parte superior muestra opciones de navegación del conjunto de datos e información básica como el autor y las etiquetas; en el centro, el área Dataset Viewer muestra parte de los datos de entrenamiento del split 1 del conjunto de datos, incluidos campos como action, observation_state y timestamp y sus valores, y también marca el tamaño de un registro, el número total de registros y el tamaño total. En la parte inferior también menciona modelos relacionados entrenados con estos datos.](../../en/images/d28-05.png)

![La imagen muestra la página del conjunto de datos soarm_amazing_hand_pick en la plataforma Hugging Face. En la parte superior hay un cuadro de búsqueda y una barra de navegación para buscar modelos, conjuntos de datos, etc. La sección de información del conjunto de datos muestra la organización propietaria Juxi - Technology y etiquetas como robotics e imitation-learning. En la pestaña "Files and versions" enumera carpetas como data, meta y videos y el archivo README.md, mostrando el responsable de la subida, el método de subida y la hora, como "Upload README.md with huggingface_hub". Esta imagen se relaciona con la visualización de un conjunto de datos de HuggingFace y presenta visualmente los archivos y las versiones del conjunto de datos.](../../en/images/d28-06.png)
