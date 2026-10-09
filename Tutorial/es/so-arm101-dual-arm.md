[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | Español | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# Tutorial de doble brazo SO-ARM101

## Introducción

Esta guía recorre el flujo de trabajo completo para entrenar un sistema robótico SO-ARM de doble brazo con LeRobot, incluido el cableado del hardware, la calibración de los dos brazos, la teleoperación con dos brazos, la grabación y gestión del conjunto de datos, el entrenamiento de la política ACT y el despliegue en el robot real. Siguiendo esta guía, puedes utilizar dos brazos leader y dos brazos follower para recopilar datos de demostración, entrenar una política de aprendizaje por imitación y ejecutarla en los brazos reales.

Primero, conecta todo como se indica a continuación

| Rol | Puerto |
|-|-|
| Follower izquierdo | /dev/ttyACM0 |
| Follower derecho | /dev/ttyACM1 |
| Leader izquierdo | /dev/ttyACM2 |
| Leader derecho | /dev/ttyACM3 |

El tipo de follower es so101_follower y el tipo de leader es so101_leader (en LeRobot, so100_leader y so101_leader comparten la misma implementación).

## Requisitos previos

### 0.1 Instalar dependencias

Para la configuración del entorno, consulta el tutorial de SO-ARM:

### 0.2 Permisos de USB

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## Calibración (paso crítico)

### 1.1 Calibrar el follower izquierdo

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 Calibrar el follower derecho

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 Calibrar el leader izquierdo

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 Calibrar el leader derecho

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

Tras la calibración, los archivos se guardan en:

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> Nota sobre los nombres de directorio: so101_follower y so100_follower, así como so101_leader y so100_leader, comparten la misma implementación, por lo que los directorios se unifican como so_follower / so_leader. El leader es un teleoperador, por lo que sus archivos de calibración están en teleoperators/ en lugar de robots/.

### (Opcional) Si calibrabas antes con otros IDs

Por ejemplo, si antes usabas my_awesome_follower_arm1, my_awesome_follower_arm2, etc., puedes copiar los archivos de calibración:

```Bash
CAL_DIR=~/.cache/huggingface/lerobot/calibration

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm1.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_left.json

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm2.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_right.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm3.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_left.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm4.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_right.json
```

---

## Teleoperación con dos brazos

### 2.1 Sin cámaras

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### 2.2 Con cámaras

Puedes usar lerobot-find-cameras opencv para comprobar los índices de las cámaras y añadir o quitar cámaras como quieras.

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### Consejos de seguridad

- Vigila el entorno y evita colisiones entre los brazos follower.

## Grabar un conjunto de datos

### 3.1 Guardar en local (sin subir al Hub)

Añade --dataset.root (el directorio en el que se escriben los datos) y --dataset.push_to_hub=false, y añade --dataset.no_stamp=true para mantener estable el nombre del conjunto de datos (de lo contrario, se añade automáticamente una marca de tiempo al repo_id, y la posterior reanudación/reproducción/entrenamiento no lo encontrarán).

> Nota: el repo_id debe contener / (en la forma nombre_de_usuario/nombre_del_conjunto_de_datos); un conjunto de datos local no se sube realmente.

```Bash
lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> La codificación de vídeo ya es libsvtav1 de forma predeterminada, así que no hace falta especificarla; para personalizarla, usa un parámetro anidado como --dataset.rgb_encoder.vcodec=h264.

Los datos se guardan en ./datasets/bi_so101_task/, con esta estructura:

```Bash
├── meta/
│   ├── info.json         # Información del conjunto de datos (fps, formas de las características, etc.)
│   ├── episodes/         # Metadatos por episodio (chunk-000/...)
│   ├── stats.json        # Estadísticas de normalización para cada característica
│   └── tasks.parquet     # Texto de la tarea → task_index
├── data/                 # Datos de características por fotograma (chunk-*.parquet)
└── videos/               # Un subdirectorio por cámara (chunk-*.mp4)
```

### 3.2 Subir al Hugging Face Hub

Si quieres la subida automática, mantén HF_USER y elimina root y push_to_hub=false (la subida es el comportamiento predeterminado). Mantén los puertos y los índices de las cámaras coherentes con la tabla de cableado:

```Bash
export HF_USER=your_hf_username

lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> El nombre del repositorio del Hub subido es ${HF_USER}/bi_so101_task, que coincide con el repo_id utilizado para el entrenamiento basado en el Hub en el punto 4.2 de más abajo. Primero se guarda una copia local en ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/.

### 3.3 Continuar la grabación (reanudar)

Si la grabación se cerró de forma inesperada (por ejemplo, saliste con el botón derecho mientras estabas en la fase de reinicio), o si quieres terminar la recopilación en varias sesiones, usa --resume para seguir añadiendo episodios al mismo conjunto de datos.

**Nota**:

- Debes añadir --resume=true, de lo contrario LeRobotDataset.create() da error porque el directorio ya existe.
- En el comando de reanudación, --dataset.root y --dataset.repo_id deben coincidir exactamente con la primera grabación (3.1) (la reanudación requiere un root explícito).
- --dataset.num_episodes es **cuántos episodios grabar esta vez**, no el objetivo total. Por ejemplo, si ya grabaste 15 y quieres 50 en total, escribe 35.
- Al salir, intenta hacerlo durante la grabación de un episodio o justo después de que termine de forma natural; evita salir durante la fase "Reset the environment" (provoca que un episodio vacío no se guarde).

```Bash
lerobot-record \
  --resume=true \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=35 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

### 3.4 Reproducción y eliminación de episodios

#### Reproducir un episodio concreto

```Bash
lerobot-replay \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.episode=24
```

> episode es un índice basado en 0, así que 24 significa el episodio número 25.

#### Eliminar un episodio concreto

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

La eliminación reescribe el conjunto de datos en el mismo lugar, y los datos originales se copian a ./datasets/bi_so101_task_old/. Una vez que hayas confirmado que el nuevo conjunto de datos es correcto, puedes eliminar manualmente la copia de seguridad:

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### Eliminar todo el conjunto de datos

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## Entrenamiento de ACT

### 4.1 Entrenar desde un conjunto de datos local

```Bash
lerobot-train \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=60000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> --dataset.root apunta al directorio del conjunto de datos grabado en 3.1 (el repo_id debe coincidir con el utilizado al grabar). Si el directorio --output_dir ya existe, lanza FileExistsError de inmediato: usa un directorio de salida nuevo o añade --resume=true para continuar el entrenamiento.

### 4.2 Entrenar desde el Hugging Face Hub

```Bash
export HF_USER=your_hf_username

lerobot-train \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=100000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> El comando anterior usa los parámetros predeterminados de ACT (chunk_size=100, dim_model=512, etc.).
> 
> El repo_id debe coincidir con el nombre del repositorio utilizado al subir en 3.2 (3.2 añade --dataset.no_stamp=true, así que el nombre del repositorio queda fijado como \${HF_USER}/bi_so101_task). No se necesita --dataset.root para el entrenamiento; se descarga automáticamente desde el Hub.

## Despliegue en el robot real

> Nota: lerobot-record es solo para recopilar datos de demostración. Usa lerobot-rollout para desplegar una política entrenada: la versión actual de lerobot-record ya no acepta --policy.path y también rechaza los nombres de conjunto de datos con el prefijo eval\_.

### 5.1 Evaluación in situ (sin grabar datos)

```Bash
lerobot-rollout \
  --strategy.type=base \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --task="Pick the cube with left arm and hand it to right arm" \
  --duration=60 \
  --display_data=true
```

- --duration es el número de segundos de ejecución; 0 significa sin límite de tiempo.
- Para tomar el control o detener a mitad de ejecución, añade --interactive=true y usa comandos como /stop y /reset en el terminal.

### 5.2 Evaluar y grabar datos (en local)

Usa la estrategia episódica (se comporta como el antiguo lerobot-record: graba por episodios con una fase de reinicio):

```Bash
lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=juxi/rollout_bi_so101_task \
  --dataset.root=./datasets/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

> El nombre de un conjunto de datos de despliegue debe empezar por rollout\_ (requisito estricto de la versión actual). Al grabar en local, añade --dataset.root y --dataset.no_stamp=true para evitar que se añada una marca de tiempo al nombre del directorio.

### 5.3 Subir los datos de evaluación al Hugging Face Hub

```Bash
export HF_USER=your_hf_username

lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=${HF_USER}/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

## Preguntas frecuentes

| Problema | Causa | Solución |
|-|-|-|
| La teleoperación pide recalibrar | bi_so_follower no encuentra archivos de calibración con el sufijo \_left / \_right | Recalibra con IDs que incluyan _left / \_right, o copia los archivos de calibración existentes |
| No se puede arrastrar el brazo leader | El par del Leader no está desactivado | Recalibra o comprueba el motor |
| Al reanudar la grabación se informa de que el directorio ya existe | No se añadió --resume=true | Añade --resume=true al comando lerobot-record |
| --resume=true da error y exige un root | La reanudación requiere un directorio de conjunto de datos explícito | Añade --dataset.root=./datasets/bi_so101_task al comando de reanudación, igual que en la primera grabación |
| El nombre del directorio del conjunto de datos tiene una marca de tiempo extra, por lo que la reproducción/el entrenamiento no pueden encontrarlo | No se configuró no_stamp al grabar, así que se añadió una marca de tiempo al repo_id | Añade --dataset.no_stamp=true al grabar/reanudar |
| --dataset.vcodec=... informa de que el parámetro no existe | Es un parámetro antiguo; el parámetro de codificación de vídeo ahora está anidado | Usa --dataset.rgb_encoder.vcodec=h264 en su lugar (el valor predeterminado ya es libsvtav1) |
| Durante el despliegue, lerobot-record informa de un error de --policy.path / eval\_ | La versión actual de lerobot-record ya no incluye el despliegue de políticas | Usa lerobot-rollout --strategy.type=episodic para el despliegue, con nombres de conjunto de datos que empiecen por rollout_ |
| Los brazos izquierdo y derecho están intercambiados | Configuración de puertos incorrecta | Intercambia left_arm_config.port y right_arm_config.port |
| El entrenamiento no encuentra el conjunto de datos | No se especificó un root para el conjunto de datos local | Añade --dataset.root=./datasets/xxx al entrenar |
| El conjunto de datos se sube automáticamente | No se configuró push_to_hub=false | Añade --dataset.push_to_hub=false al grabar |
| Al salir, informa de You must add one or several frames before calling add_episode | Saliste durante la fase de reinicio, así que el episodio actual no tiene fotogramas | No afecta a los datos ya grabados; usa --resume=true para seguir recopilando |
