[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | Italiano | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# Passo 2: sostituire i file (adattamento al 7DOF)

## 1. Se hai clonato il repository ufficiale, quali file devi sostituire/modificare

### Opzione A: usare direttamente il codice di questo repository (consigliato)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Opzione B: sostituzione manuale dopo aver clonato il lerobot ufficiale con git

Sovrascrivi **3 file** della clone ufficiale con quelli di questo repository:

| File di questo repository (origine) | Da sovrascrivere in (destinazione) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | File con lo stesso nome nella clone ufficiale |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | File con lo stesso nome nella clone ufficiale |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> Premessa: la tua clone ufficiale ha la stessa struttura della base di questo repository (versione lerobot 2026-09).
> 
> Se le versioni differiscono molto, **non sovrascrivere l'intero file**: passa invece alle due "modifiche manuali" descritte qui sotto.

### Modifiche manuali in caso di versioni non allineate (solo due punti)

**① Dizionario dei motori** (uno in `so_follower.py` e uno in `so_leader.py`, con contenuto identico) — sostituisci il

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

con

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # servo nuovo, rotazione sinistra/destra
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # motore di rollio n. 5 originale, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② Logica di calibrazione** (`calibrate()` di ciascuno dei due file) — elimina il caso speciale del "giunto a giro completo": sostituisci

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

con una sola riga, che registra i limiti reali di tutti i giunti:

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> Il motivo: la versione originale trattava `wrist_roll` (il rollio attorno all'asse dell'avambraccio) come un giunto in grado di ruotare a giro completo (0\~4095) e ne forzava l'intero range. Dopo la conversione a 7-DOF, i giunti del polso n. 5/6 (yaw / roll) **sono entrambi limitati meccanicamente e non possono fare un giro completo**: impostare a mano il giro completo farebbe inviare al codice comandi di giunto verso angoli che la meccanica non può raggiungere, con rischio di danni. Ora durante la calibrazione ogni motore registra manualmente i propri min/max reali.

### Nota sui due bracci follower

`bi_so_follower` / `bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) si limitano ad avvolgere il braccio singolo aggiungendo il prefisso `left_`/`right_` e **non contengono definizioni di motori**. Basta che i **file del braccio singolo qui sopra** siano corretti perché i comandi a due bracci (`--robot.type=bi_so_follower`) siano automaticamente a 7-DOF.
