[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | Español | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# Configuración de un entorno de entrenamiento con GPU en la nube

## Desactiva el proxy de red de tu ordenador

De lo contrario, es posible que no puedas abrir la línea de comandos de Jupyter

## Inicia sesión en la plataforma de GPU en la nube Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

Únete al grupo de usuarios, di al servicio de atención al cliente que eres fan de Zihao de Tongji y consigue un cupón

## Arranca una instancia de GPU en la nube

## Instala y configura el entorno

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## Inicia sesión en wandb

```Shell
wandb login
Copia y pega la API Key y pulsa Enter
```

![La imagen muestra el proceso de inicio de sesión en W&B (Weights & Biases) mediante el comando `wandb login` en el entorno de Lerobot. Tras ejecutar el comando, W&B pide iniciar sesión en https://api.wandb.ai, indica que se puede desplegar un servidor local de W&B y guía para encontrar la clave de API. A continuación, W&B pide pegar la clave de API y pulsar Enter, y también avisa de que no se encontró el archivo .netrc y de que se está creando. Por último, W&B muestra que ha iniciado sesión como tommyzihao, con la sesión iniciada en https://api.wandb.ai. Esta imagen se relaciona con el paso "Inicia sesión en wandb" del documento y presenta visualmente la información de respuesta de la operación de inicio de sesión.](../../en/images/d35-01.png)

## Monta el conjunto de datos

```Shell
Copia el comando de descarga de la instancia, algo como:
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

El conjunto de datos aparece bajo el directorio `~`

## Ajusta la frecuencia de guardado de pesos (opcional)

Abre `lerobot/src/lerobot/configs/train.py`

Cambia save_freq de 20_000 a 5_000

Así obtendrás los archivos de pesos del modelo antes durante el entrenamiento
