[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | Español | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# Paso 2: reemplazar archivos (adaptación a 7DOF)

## 1. Si has clonado el repositorio oficial, qué archivos hay que reemplazar/modificar

### Opción A: usar directamente el código de este repositorio (recomendado)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Opción B: sustitución manual tras clonar el lerobot oficial con git

Copia **3 archivos** de este repositorio sobre tu clon oficial:

| Archivo de este repositorio (origen) | Sobrescribir en (destino) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | El archivo con el mismo nombre del clon oficial |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | El archivo con el mismo nombre del clon oficial |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> Requisito previo: tu clon oficial tiene la misma estructura que la base de este repositorio (versión lerobot 2026-09).
> 
> Si las versiones difieren mucho, **no sobrescribas el archivo entero**; haz en su lugar las dos "modificaciones manuales" descritas a continuación.

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
