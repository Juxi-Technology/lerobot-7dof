[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | Español | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | [한국어](../ko/09-train-model/upload-model-to-huggingface.md) | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# Subir un modelo a HuggingFace (opcional)

## Crea un repositorio de modelo

<grid>
<column width-ratio="0.354197">
![Esta imagen muestra la interfaz de usuario de HuggingFace. Aparece un icono de avatar; al hacer clic en él se despliega un menú en el que la opción "New Model" está resaltada con un recuadro rojo. La imagen se relaciona con la sección "Subir un modelo a HuggingFace (opcional)" y corresponde al paso "Crea un repositorio de modelo", presentando visualmente el punto de entrada para crear un modelo nuevo en HuggingFace y ayudando al usuario a entender cómo crear recursos de modelo en la plataforma.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![Esta imagen muestra la interfaz para crear un repositorio de modelo nuevo en el sitio de HuggingFace. El desplegable "Owner" está ajustado a "TommyZihao", el campo "Model name" contiene "lerobot_zihao_model_a" y el campo "License" contiene "mit". Debajo hay una opción "Base template" y las alternativas de tipo de repositorio "Public" y "Private". La imagen se relaciona con la sección "Crea un repositorio de modelo" y es un ejemplo de cómo rellenar los datos al crear un repositorio de modelo.](../../en/images/d46-02.png)
</column>
</grid>

## Consulta el repositorio del modelo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

De momento está vacío

![Esta imagen muestra la página del modelo "TommyZihao/lerobot_zihao_model_a" en la plataforma HuggingFace. A la izquierda hay una pestaña "Model card" para editar la tarjeta del modelo. A la derecha, la sección "Getting started with your model" explica cómo empezar a usar el modelo, incluida la forma de añadir información completa del modelo y subir archivos. Debajo, el área "Edit Model Card" permite añadir la licencia, el idioma, el modelo base y otra información. En la parte inferior, el área "Push your model files" ofrece varias formas de subir archivos del modelo, como CLI, Python, Git, HTTPS y SSH. La imagen se relaciona con la subida de un modelo a HuggingFace y muestra los controles de la página.](../../en/images/d46-03.png)

![Esta imagen muestra la página del repositorio del modelo TommyZihao/lerobot_zihao_model_a en la plataforma HuggingFace. La página indica que el tamaño del modelo es de 1.54 KB, muestra un colaborador y un historial de 1 commit, realizado hace 9 minutos. También lista los archivos .gitattributes y README.md, de 1.52 KB y 24 Bytes respectivamente, igualmente del commit inicial, también de hace 9 minutos. La imagen se relaciona con la subida de un modelo a HuggingFace y muestra cómo queda la página una vez subido el modelo.](../../en/images/d46-04.png)

## Sube el modelo

Crea un archivo `upload_model.py` con el siguiente contenido

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

Ejecútalo

```Shell
python upload_model.py
```

![Esta imagen muestra la salida de ejecutar el comando `python upload_model.py` desde la línea de comandos. Muestra el progreso de procesamiento de archivos al 34% y el de subida de datos nuevos también al 34%, y lista el progreso de subida de los dos archivos `d_model/model.safetensors` y `tokenizer_processor.safetensors`, ambos al 92%. La imagen se relaciona con la subida de un modelo a HuggingFace y muestra visualmente el progreso al subir los archivos del modelo.](../../en/images/d46-05.png)

## Consulta el repositorio del modelo

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![Esta imagen muestra la página del repositorio de TommyZihao, el modelo lerobot_zihao_model_a, en HuggingFace. La página indica que la licencia del modelo es mit, muestra un colaborador y un historial de 2 commits. En el centro lista varios archivos, como README.md, config.json y model.safetensors, cada uno con el texto "Upload folder using huggingface_hub" a su derecha, lo que indica que estos archivos se subieron mediante huggingface_hub. La imagen se relaciona con la subida de un modelo a HuggingFace y muestra visualmente cómo se almacenan los archivos del modelo en HuggingFace.](../../en/images/d46-06.png)

Ya están los archivos del modelo en su sitio
