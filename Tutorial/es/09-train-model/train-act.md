[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | Español | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# Línea de comandos de entrenamiento - ACT (recomendado para principiantes)

## Documentación de referencia

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## Por qué empezar con el algoritmo ACT

ACT es el primer modelo más recomendado para entrenar cuando te inicias en LeRobot. Sus ventajas son:

- El modelo es muy ligero, con solo 80 millones de parámetros entrenables
- El entrenamiento converge rápido y la inferencia también es rápida
- Puedes ver resultados tras solo una hora de entrenamiento en una única GPU
- El archivo del modelo ACT ocupa unos 200 MB, lo que facilita su almacenamiento y transferencia
- Normalmente basta con recoger unos 30 episodios de datos
- Se puede desplegar para inferencia en un equipo Ubuntu, un Mac, un PC Windows e incluso una Raspberry Pi
- La inferencia en un robot real funciona bastante bien y es más que suficiente para tareas sencillas como recoger, dar la mano o colocar un bolígrafo
- El algoritmo ACT ya viene integrado en el entorno base de LeRobot, por lo que no hacen falta bibliotecas adicionales

## Línea de comandos

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## Notas sobre la línea de comandos

Una continuación de línea `\` solo puede tener un espacio delante y ninguno detrás

Los parámetros mostrados en rojo deben revisarse o cambiarse antes de cada ejecución

| Parámetro de la línea de comandos | Descripción |
|-|-|
| --dataset.repo_id | Repo_ID del conjunto de datos de HuggingFace |
| --dataset.root | Ruta local del conjunto de datos |
| --dataset.revision | Versión del conjunto de datos, especificada al subirlo a HuggingFace |
| --dataset.streaming | El conjunto de datos es local, por lo que debe ser `false`, ya que los datos ya están en disco y no hace falta una lectura en streaming |
| --dataset.split | Tiene el valor predeterminado `train`, lo que significa que se usa el conjunto de datos completo como conjunto de entrenamiento |
| --policy.type | El algoritmo que se va a entrenar, como act, smolvla, diffusion, pi0, wallx |
| --output_dir | Directorio donde se guarda la estructura de salida |
| --job_name | Nombre de este trabajo de entrenamiento |
| --policy.device | Dispositivo de cómputo |
| --wandb.enable | Activa la visualización con wandb |
| --wandb.project | Nombre del proyecto de wandb |
| --policy.push_to_hub | Sube el modelo entrenado a HuggingFace |
| --steps | Número de pasos de entrenamiento |
| --batch_size | Cantidad de datos introducidos por paso; redúcelo si te quedas sin memoria de GPU |
|  |  |

## Proceso de entrenamiento

<grid>
<column width-ratio="0.357753">
![Esta imagen muestra un ejemplo de uso del comando de entrenamiento `lerobot-train` desde la línea de comandos. El comando ajusta varios parámetros, como `--dataset.repo_id` y `--dataset.root`, para especificar los detalles del conjunto de datos, fija `--policy.type` en `act` y `--output_dir` en el directorio de salida `outputs/lerobot_train/output_a`, junto con otros parámetros como `--job_name` y `--policy.device`. También enumera los valores predeterminados de parámetros como `--dataset.split` y `--policy.push_to_hub`. La imagen guarda una relación estrecha con el contexto y muestra visualmente cómo se configuran los parámetros del comando de entrenamiento.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![Esta imagen muestra la salida de la línea de comandos durante el entrenamiento. Presenta detalles del entrenamiento del modelo, como los ajustes de scheduler, steps y use_policy_training_preset, junto con parámetros relacionados con el conjunto de datos. También muestra información como el número de parámetros del modelo y la pérdida; por ejemplo, num_total_params de 55917096 (52M) y una pérdida de 0.626. Debajo aparece información de descarga de archivos, como la descarga de "https://download.pytorch.org/models/resnet18-f37072fd.pth" al directorio /home/featurize/.cache/torch/hub/checkpoints. La imagen se relaciona con la línea de comandos de entrenamiento descrita en el contexto y muestra visualmente lo que la línea de comandos emite durante el entrenamiento.](../../en/images/d36-02.png)
</column>
</grid>

![Esta imagen muestra la información de registro generada durante el entrenamiento. El registro recoge varios pasos de entrenamiento en orden cronológico, incluidos la hora, el número de iteración, la pérdida y la tasa de aprendizaje; por ejemplo, a las 15:11:53 del 14 de enero de 2024 el número de iteración era 131k y la pérdida era 0.368. Aquí `INFO` es el tipo de registro, `train` la fase de entrenamiento, `step` el número de iteración, `loss` el valor de la pérdida y `lr` la tasa de aprendizaje. La imagen se relaciona con la sección de notas sobre la línea de comandos del documento y presenta visualmente los datos clave de la ejecución de entrenamiento.](../../en/images/d36-03.png)

El archivo del modelo ocupa unos 300 MB
