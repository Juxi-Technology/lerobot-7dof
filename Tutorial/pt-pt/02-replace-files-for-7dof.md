[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | Português (PT)

# Passo 2: Substituir ficheiros (para 7-DOF)

## 1. Se Clonou o Repositório de Código Oficial, Que Ficheiros É Preciso Substituir/Alterar

### Opção A: Utilizar Diretamente o Código deste Repositório (Recomendado)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Opção B: Substituir Manualmente Após Clonar o lerobot Oficial

Copie **3 ficheiros** deste repositório para cima da sua clonagem oficial:

| Ficheiro deste repositório (origem) | Substituir em (destino) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | O ficheiro com o mesmo nome na clonagem oficial |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | O ficheiro com o mesmo nome na clonagem oficial |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> Pré-requisito: a sua clonagem oficial tem a mesma estrutura que a base deste repositório (a versão lerobot 2026-09).
> 
> Se as versões forem muito diferentes, **não substitua ficheiros inteiros** — faça antes as duas alterações de "modificação manual" abaixo.

### Modificação Manual Quando as Versões São Diferentes (Apenas Dois Pontos)

**① Dicionário de motores** (uma cópia em `so_follower.py` e outra em `so_leader.py`, com conteúdo idêntico) — altere

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

para

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # servo novo adicionado, rotação esquerda/direita
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # o motor de roll original do servo 5, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② Lógica de calibração** (o `calibrate()` de cada um dos dois ficheiros) — remova o caso especial da "articulação de volta completa": altere

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

para uma única linha que regista o intervalo real de cada articulação:

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> Porquê: o código original codifica de forma fixa o `wrist_roll` (rotação em torno do eixo do antebraço) como uma articulação capaz de dar uma volta completa (0\~4095) e define o seu intervalo como o intervalo total. Após a conversão para 7-DOF, as duas articulações do pulso (yaw / roll, servos 5/6) ficam **ambas limitadas mecanicamente e não conseguem dar uma volta completa**; forçar um intervalo de volta completa faz com que o código envie comandos de articulação para ângulos que a mecânica não consegue atingir, com risco de danos. Agora, durante a calibração, é registado manualmente o mínimo/máximo real de cada motor.

### Nota sobre os Dois Braços Follower

`bi_so_follower` / `bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) limitam-se a envolver um único braço e a acrescentar um prefixo `left_`/`right_`; **não contêm definições de motores**. Desde que os **ficheiros de braço único acima** estejam feitos, os comandos de braço duplo (`--robot.type=bi_so_follower`) são automaticamente 7-DOF.
