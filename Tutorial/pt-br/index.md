[English](../en/index.md) | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | Português (BR) | [Português (PT)](../pt-pt/index.md)

<title>Tutorial do SO-ARM101 7-DOF</title>

# SO-ARM101 7-DOF · Preparação Antes de Executar

> Este guia é para quem **converteu um SO-ARM101 de 6 servos para 7 servos** e agora quer executar o fluxo completo do LeRobot (calibração → gravação → treinamento → implantação) nele.  
> Código correspondente: este repositório (`lerobot-7dof`), um fork do lerobot oficial que altera apenas a configuração dos motores relacionados ao SO.

---

## 0. Primeiro, Confirme o Seu Braço Robótico

7 servos (todos STS3215), o mapeamento entre os IDs dos servos e as articulações:

| **ID do servo** | **Nome da articulação** | **Descrição** |
|-|-|-|
| 1 | `shoulder_pan` | Rotação horizontal do ombro |
| 2 | `shoulder_lift` | Elevação do ombro |
| 3 | `elbow_flex` | Flexão do cotovelo |
| 4 | `wrist_flex` | Inclinação do pulso (flexão para cima/para baixo) |
| 5 | `wrist_yaw` | Guinada do pulso (rotação esquerda/direita de cerca de 90°) · **o servo recém-adicionado desta vez** (inserido entre os antigos servos 4 e 5) |
| 6 | `wrist_roll` | Rotação do pulso · originalmente o motor de rolagem do servo 5, ID 5→6; a peça impressa não mudou e o nome permanece o mesmo |
| 7 | `gripper` | Garra · originalmente ID=6, deslocado para 7 após a alteração |

Ordem dos dados das articulações (a ordem das dimensões das articulações de `action` / `observation.state` nos arquivos Parquet após a gravação):  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`.

⚠️ Observação: **os dados, os arquivos de calibração e os modelos já treinados da versão de 6 servos são incompatíveis com este repositório**, então você deve refazer tudo o que segue do zero.

---

## 1. Se Você Clonou o Repositório de Código Oficial, Quais Arquivos Precisam Ser Substituídos/Modificados

### Opção A: Usar Diretamente o Código Deste Repositório (Recomendado)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Opção B: Substituir Manualmente Após Clonar o lerobot Oficial

Copie **3 arquivos** deste repositório sobre o seu clone oficial:

| Arquivo deste repositório (origem) | Substituir em (destino) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | O arquivo de mesmo nome no clone oficial |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | O arquivo de mesmo nome no clone oficial |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | O arquivo de mesmo nome no clone oficial (**apenas correção de comentários**, sem impacto funcional; pode não trocar) |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <official-clone>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <official-clone>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> Pré-requisito: o seu clone oficial tem a mesma estrutura da base deste repositório (a versão lerobot 2026-08). Se as versões forem muito diferentes, **não substitua arquivos inteiros** — em vez disso, faça as duas alterações de "modificação manual" abaixo.

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

`bi_so_follower / bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) apenas envolvem um braço único e adicionam o prefixo `left_`/`right_`, **sem conter definições de motores**. Desde que os **arquivos de braço único acima** estejam prontos, os comandos de braço duplo (`--robot.type=bi_so_follower`) já são automaticamente 7-DOF.
