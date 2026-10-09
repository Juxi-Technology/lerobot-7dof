[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | Português (BR) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# Etapa 2: Substituir Arquivos (para 7-DOF)

## 1. Se Você Clonou o Repositório de Código Oficial, Quais Arquivos Precisam Ser Substituídos/Modificados

### Opção A: Usar Diretamente o Código Deste Repositório (Recomendado)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Opção B: Substituir Manualmente Após Clonar o lerobot Oficial

Copie **3 arquivos** deste repositório sobre o seu clone oficial:

| Arquivo deste repositório (origem) | Substituir em (destino) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | O arquivo de mesmo nome no clone oficial |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | O arquivo de mesmo nome no clone oficial |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> Pré-requisito: o seu clone oficial tem a mesma estrutura da base deste repositório (a versão lerobot 2026-09).
> 
> Se as versões forem muito diferentes, **não substitua o arquivo inteiro** — em vez disso, faça as duas alterações de "modificação manual" abaixo.

### Modificação Manual Quando as Versões São Diferentes (Apenas Dois Lugares)

**① Dicionário de motores** (uma cópia em `so_follower.py` e uma em `so_leader.py`, com conteúdo idêntico) — altere o

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

para

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # servo recém-adicionado, rotação esquerda/direita
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # o motor de rolagem do antigo servo 5, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② Lógica de calibração** (o `calibrate()` em cada um dos dois arquivos) — remova o caso especial de "articulação de volta completa": altere o

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

para uma única linha que registra o intervalo real de cada articulação:

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> Por quê: o código original fixa `wrist_roll` (rolagem em torno do eixo do antebraço) como uma articulação que pode girar uma volta completa (0\~4095) e define seu intervalo como o curso total. Após a conversão para 7-DOF, as duas articulações do pulso (yaw / roll, servos 5/6) **estão ambas limitadas mecanicamente e não conseguem girar uma volta completa**; forçar um intervalo de volta completa faz o código enviar comandos de articulação para ângulos que a mecânica não alcança, com risco de danos. Agora, durante a calibração, o min/max real de cada motor é registrado manualmente.

### Observação Sobre os Braços Follower Duplos

`bi_so_follower` / `bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) apenas envolvem um braço único e adicionam o prefixo `left_`/`right_`, **sem conter definições de motores**. Desde que os **arquivos de braço único acima** estejam prontos, os comandos de braço duplo (`--robot.type=bi_so_follower`) já são automaticamente 7-DOF.
