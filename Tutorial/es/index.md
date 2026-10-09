[English](../en/index.md) | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | Español | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>SO-ARM101 tutorial del brazo robótico de 7 ejes</title>

# SO-ARM101 7-DOF · Preparación antes de empezar

> Esta guía está dirigida a quienes han **convertido el SO-ARM101 de 6 servos a 7 servos** y ahora quieren ejecutar con LeRobot el flujo completo (calibración → grabación → entrenamiento → despliegue).  
> Código correspondiente: este repositorio (`lerobot-7dof`), un fork del lerobot oficial en el que solo se ha modificado la configuración de los motores relacionada con el SO.

---

## 0. Primero, confirma tu brazo robótico

7 servos (todos STS3215). Correspondencia entre los ID de los servos y las articulaciones:

| **ID del servo** | **Nombre de la articulación** | **Descripción** |
|-|-|-|
| 1 | `shoulder_pan` | Rotación horizontal del hombro |
| 2 | `shoulder_lift` | Elevación del hombro |
| 3 | `elbow_flex` | Flexión del codo |
| 4 | `wrist_flex` | Inclinación de la muñeca (flexión arriba/abajo) |
| 5 | `wrist_yaw` | Guiñada de la muñeca (rotación izquierda/derecha de unos 90°) · **el servo nuevo de esta versión** (insertado entre los antiguos servos 4 y 5) |
| 6 | `wrist_roll` | Balanceo de la muñeca · era el motor de balanceo n.º 5, ID 5→6; la pieza impresa no cambia y el nombre se mantiene |
| 7 | `gripper` | Pinza · antes ID=6, tras el cambio pasa a 7 |

Orden de los datos de las articulaciones (el orden de las dimensiones de las articulaciones en `action` / `observation.state` del archivo Parquet tras la grabación):  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`.

⚠️ Atención: **los datos, los archivos de calibración y los modelos ya entrenados de la versión de 6 servos no son compatibles con este repositorio**, por lo que debes rehacer todo lo que sigue desde cero.

---

## 1. Si has clonado el repositorio oficial, qué archivos hay que reemplazar/modificar

### Opción A: usar directamente el código de este repositorio (recomendado)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Opción B: sustitución manual tras clonar el lerobot oficial con git

Copia **3 archivos** de este repositorio sobre tu clon oficial:

| Archivo de este repositorio (origen) | Sobrescribir en (destino) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | El archivo con el mismo nombre del clon oficial |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | El archivo con el mismo nombre del clon oficial |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | El archivo con el mismo nombre del clon oficial (**solo corrección de comentarios**, no afecta a la funcionalidad, puedes no sustituirlo) |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <clon-oficial>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <clon-oficial>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> Requisito previo: tu clon oficial tiene la misma estructura que la base de este repositorio (versión lerobot 2026-08). Si las versiones difieren mucho, **no sobrescribas el archivo entero**; haz en su lugar las dos "modificaciones manuales" descritas a continuación.

### Modificación manual cuando las versiones no coinciden (solo dos puntos)

**① Diccionario de motores** (una copia en `so_follower.py` y otra en `so_leader.py`, con contenido idéntico): cambia el

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

por

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # servo nuevo, rotación izquierda/derecha
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # el motor de balanceo n.º 5 original, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② Lógica de calibración** (el `calibrate()` de cada uno de los dos archivos): elimina el caso especial de la "articulación de giro completo": cambia

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

por una sola línea, que registra el rango real de todas las articulaciones:

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> Por qué: la versión original trataba `wrist_roll` (el balanceo alrededor del eje del antebrazo) como una articulación capaz de girar una vuelta completa (0\~4095) y le asignaba a la fuerza el rango completo. Tras la conversión a 7-DOF, las articulaciones de la muñeca n.º 5/6 (yaw / roll) **están ambas limitadas mecánicamente y no pueden dar una vuelta completa**; forzar el giro completo haría que el código enviara comandos de articulación a ángulos que la mecánica no puede alcanzar, con riesgo de dañarla. Ahora, durante la calibración, cada motor registra manualmente sus mínimos/máximos reales.

### Nota sobre los dos brazos seguidores

`bi_so_follower` / `bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) solo envuelven el brazo individual añadiendo el prefijo `left_`/`right_` y **no contienen definiciones de motores**. Basta con que los **archivos del brazo individual de arriba** estén bien para que los comandos de dos brazos (`--robot.type=bi_so_follower`) sean automáticamente 7-DOF.
