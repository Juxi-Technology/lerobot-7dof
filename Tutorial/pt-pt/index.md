[English](../en/index.md) | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | Português (PT)

<title>SO-ARM101 7-DOF Tutorial</title>

# SO-ARM101 7-DOF · Preparação Antes de Executar

> Este guia destina-se a quem **converteu um SO-ARM101 de 6 servos para 7 servos** e agora quer percorrer todo o fluxo do LeRobot (calibração → gravação → treino → implementação) com ele.  
> Código correspondente: este repositório (`lerobot-7dof`), um fork do lerobot oficial que altera apenas a configuração dos motores relacionados com o SO.

---

## 0. Primeiro, Confirme o Seu Braço Robótico

7 servos (todos STS3215). O mapeamento entre os IDs dos servos e as articulações:

| **ID do servo** | **Nome da articulação** | **Descrição** |
|-|-|-|
| 1 | `shoulder_pan` | Rotação horizontal do ombro |
| 2 | `shoulder_lift` | Elevação do ombro |
| 3 | `elbow_flex` | Flexão do cotovelo |
| 4 | `wrist_flex` | Inclinação do pulso (flexão para cima/para baixo) |
| 5 | `wrist_yaw` | Guinada do pulso (rotação esquerda/direita de cerca de 90°) · **o servo novo adicionado desta vez** (inserido entre os servos 4 e 5 originais) |
| 6 | `wrist_roll` | Rotação do pulso (roll) · originalmente o motor de roll do servo 5, ID 5→6; a peça impressa não mudou e o nome mantém-se |
| 7 | `gripper` | Pinça · originalmente ID=6, deslocado para 7 após a alteração |

Ordem dos dados das articulações (a ordem das dimensões das articulações de `action` / `observation.state` nos ficheiros Parquet após a gravação):  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`.

⚠️ Nota: **os dados, os ficheiros de calibração e os modelos já treinados da versão de 6 servos são incompatíveis com este repositório**, pelo que tem de refazer tudo o que se segue desde o início.

---

## 1. Se Clonou o Repositório de Código Oficial, Que Ficheiros É Preciso Substituir/Alterar

### Opção A: Utilizar Diretamente o Código deste Repositório (Recomendado)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Opção B: Substituir Manualmente Após Clonar o lerobot Oficial

Copie **3 ficheiros** deste repositório para cima da sua clonagem oficial:

| Ficheiro deste repositório (origem) | Substituir em (destino) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | O ficheiro com o mesmo nome na clonagem oficial |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | O ficheiro com o mesmo nome na clonagem oficial |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | O ficheiro com o mesmo nome na clonagem oficial (**apenas comentários**, sem impacto funcional; opcional) |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <clonagem-oficial>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <clonagem-oficial>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> Pré-requisito: a sua clonagem oficial tem a mesma estrutura que a base deste repositório (a versão lerobot 2026-08). Se as versões forem muito diferentes, **não substitua ficheiros inteiros** — faça antes as duas alterações de "modificação manual" abaixo.

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
