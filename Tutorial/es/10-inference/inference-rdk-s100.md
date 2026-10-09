[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | Español | [Français](../fr/10-inference/inference-rdk-s100.md) | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# Inferencia en D-Robotics RDK S100

Para consultar el flujo de implementación detallado, haz referencia a este enlace<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy Full Workflow Document" type="doc"></cite>



## Despliegue de extremo a extremo del modelo ACT en RDK S100/S100P

Esta sección te guía por el ciclo completo de despliegue del modelo ACT en el hardware de la serie D-Robotics RDK S100. Todo el proceso tiene tres etapas principales: **exportación del modelo**, **compilación de la cuantización** y **ejecución en la placa**.

<callout emoji="💡">
**Requisitos previos:**
- **Máquina de desarrollo (Host):** se usa para ejecutar los pasos 1 y 2, normalmente tu máquina de entrenamiento del modelo (necesita un rendimiento decente y tener Docker instalado).
- **Placa (Edge):** la D-Robotics RDK S100/S100P, se usa para ejecutar el paso 3.
- **Cadena de herramientas:** este artículo se apoya en el repositorio `rdk_LeRobot_tools`; consulta el [repositorio de GitHub](https://github.com/D-Robotics/rdk_LeRobot_tools) para más detalles.
</callout>

<callout emoji="🚨">
**Nota importante de compatibilidad de versiones (de lectura obligatoria):** el flujo de exportación a ONNX actual de `rdk_LeRobot_tools` es totalmente compatible con los **conjuntos de datos de LeRobot v2.1**. Como la última v3.0 cambia la estructura de datos, se **recomienda encarecidamente** que, antes de hacer el trabajo de esta sección, cambies el repositorio principal `lerobot` al commit concreto compatible con v2.1, para que el flujo de exportación funcione sin problemas. 
*ID de commit recomendado:* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### Etapa 1: Exportar el modelo a formato ONNX 💻 (en la máquina de desarrollo)

Primero, necesitamos exportar el modelo **entrenado con PyTorch** a un formato intermedio (ONNX).



#### **1. Clona el repositorio de la cadena de herramientas** 

Ve a tu directorio de trabajo `lerobot` y clona la cadena de herramientas específica de RDK:

```Bash
cd lerobot

# 1. Switch to the stable version compatible with v2.1 datasets
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. Clone the D-Robotics RDK-specific toolchain
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. Configura los parámetros de exportación** 

Edita el archivo `rdk_LeRobot_tools/bpu_export_config.yaml` y ajusta la configuración para que coincida con tus rutas reales:

```YAML
dataset:
  root: "data/so101_pick_place" # absolute or relative path to your dataset
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # path to the original PyTorch model weights
type: "nash-e" # target hardware architecture; RDK S100 corresponds to nash-e / S100P corresponds to nash-m
```



#### 3. Ejecuta el script de exportación

```Bash
# Export ONNX (development machine)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **Indicador de éxito**: se crea una carpeta `bpu_export_output` en el directorio actual, que contiene el script `build_all.sh` y los datos de calibración de la cuantización necesarios más adelante.



### Etapa 2: Compilar el modelo BPU 🐳 (en un entorno Docker de la máquina de desarrollo)

Cuantizar y compilar modelos BPU de D-Robotics requiere un entorno OpenExplorer (OE). Recomendamos usar Docker para aislar el entorno.



#### **1.** **Prepara el entorno Docker y la imagen** 

Asegúrate de que Docker está instalado en la máquina de desarrollo ([guía oficial de instalación](https://docs.docker.com/engine/install/)). Descarga la imagen de CPU recomendada y cárgala:

```Bash
# Load the downloaded offline image archive
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. Inicia el contenedor de compilación**

<callout emoji="⚠️">
**Aviso de trampa**: compilar el modelo necesita una gran cantidad de memoria compartida. Asegúrate de añadir el argumento `--shm-size=15g`; de lo contrario, es muy probable que aparezcan errores de memoria IPC.
</callout>

Monta el directorio de trabajo de la máquina de desarrollo (con la carpeta que acabas de exportar) dentro del contenedor:

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(Nota: sustituye `<docker-image-name>` por el nombre real de la imagen que veas con `sudo docker images`.)



#### **3.** **Ejecuta la compilación dentro del contenedor** 

Una vez dentro del contenedor, ejecuta el script de compilación en un solo clic:

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **Comprueba los artefactos de compilación** 

Cuando termina la compilación, se crea una carpeta `bpu_output/` dentro de `bpu_export_output`. Contiene todos los archivos principales necesarios para ejecutar en la placa RDK: 

- Haz clic para ver la estructura del directorio `bpu_output/`

  - `BPU_ACTPolicy_TransformerLayers.hbm` (archivo de modelo cuantizado)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (archivo de modelo cuantizado)
  - `action_mean.npy` y varios otros parámetros de normalización del conjunto de datos
  - `camera1_mean.npy` y otros parámetros estadísticos de la cámara

---

### Etapa 3: Despliegue e inferencia en la placa 🤖 (en la RDK S100)

<callout emoji="📌">
**Comprobación de requisitos previos:**
1. La placa RDK ya tiene configurado el entorno de ejecución `D-Robotics/lerobot`, con `hbm_runtime` instalado.
2. Toda la carpeta `bpu_output/` generada en el paso anterior se ha copiado por completo a la placa RDK, mediante `scp`, una unidad USB o similar.
3. La configuración básica de teleoperación ya está hecha, lo que garantiza que el puerto serie del brazo, el puerto USB de la cámara y el archivo de calibración estén configurados correctamente.
</callout>



#### **1.** **Ejecuta la inferencia acelerada por BPU**

En el terminal de la placa RDK, ve al directorio de la cadena de herramientas e inicia el script de control:

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ Solución de problemas

Si encuentras problemas durante un despliegue real, comprueba la siguiente lista:

- **¿El brazo no se mueve?**

  - Comprueba que el dispositivo esté montado: escribe `ls /dev/ttyACM*` en el terminal y confirma que el puerto serie del brazo es correcto.
  - Comprueba los permisos: prueba a ejecutar el script de inferencia con `sudo`, o añade el usuario actual al grupo `dialout`.
- **¿Error de emisión de la cámara / imagen anómala / el brazo tiembla en el sitio?**

  - Confirma si el índice de la cámara se ha desplazado por un hot-plug, y comprueba que los parámetros de la cámara en el código coinciden con el `/dev/video*` real.
- **¿Al copiar archivos generados por el contenedor en la máquina de desarrollo da "permisos insuficientes"?**

  - Los archivos creados en un directorio montado de Docker pertenecen a root por defecto; ejecuta `sudo chown -R $USER:$USER bpu_export_output` en la máquina de desarrollo para solucionarlo.
