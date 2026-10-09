[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | Português (PT)

# Tutorial do Braço Duplo SO-ARM101

## Introdução

Este guia percorre todo o fluxo de trabalho para treinar um sistema robótico SO-ARM de braço duplo com o LeRobot, incluindo a cablagem do hardware, a calibração de braço duplo, a teleoperação de braço duplo, a gravação e gestão de conjuntos de dados, o treino de uma política ACT e a implementação no robô real. Seguindo este guia, pode usar dois braços leader e dois braços follower para recolher dados de demonstração, treinar uma política de aprendizagem por imitação e executá-la nos braços reais.

Primeiro, faça a cablagem de tudo da seguinte forma

| Função | Porta |
|-|-|
| Follower esquerdo | /dev/ttyACM0 |
| Follower direito | /dev/ttyACM1 |
| Leader esquerdo | /dev/ttyACM2 |
| Leader direito | /dev/ttyACM3 |

O tipo follower é so101_follower e o tipo leader é so101_leader (no LeRobot, so100_leader e so101_leader partilham a mesma implementação).

## Pré-requisitos

### 0.1 Instalar as dependências

Para a configuração do ambiente, consulte o tutorial do SO-ARM:

### 0.2 Permissões de USB

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## Calibração (passo crítico)

### 1.1 Calibrar o follower esquerdo

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 Calibrar o follower direito

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 Calibrar o leader esquerdo

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 Calibrar o leader direito

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

Após a calibração, os ficheiros são guardados em:

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> Nota sobre os nomes dos diretórios: so101_follower e so100_follower, bem como so101_leader e so100_leader, partilham a mesma implementação, pelo que os diretórios são unificados como so_follower / so_leader. O leader é um teleoperador, por isso os seus ficheiros de calibração ficam em teleoperators/ em vez de robots/.

### (Opcional) Se calibrou anteriormente com outros IDs

Por exemplo, se anteriormente usou my_awesome_follower_arm1, my_awesome_follower_arm2, etc., pode copiar os ficheiros de calibração:

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

## Teleoperação de braço duplo

### 2.1 Sem câmaras

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

### 2.2 Com câmaras

Pode usar lerobot-find-cameras opencv para verificar os índices das câmaras, e adicionar ou remover câmaras como quiser.

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

### Dicas de segurança

- Vigie a zona em redor e evite colisões entre os braços follower.

## Gravar um Conjunto de Dados

### 3.1 Guardar localmente (sem enviar para o Hub)

Adicione --dataset.root (o diretório onde os dados são escritos) e --dataset.push_to_hub=false, e adicione --dataset.no_stamp=true para manter o nome do conjunto de dados estável (caso contrário, é automaticamente anexado um timestamp ao repo_id, e depois a retomada/reprodução/treino não o encontrará).

> Nota: o repo_id deve conter / (na forma nome-de-utilizador/nome-do-conjunto-de-dados); um conjunto de dados local não é de facto enviado.

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

> A codificação de vídeo já é libsvtav1 por predefinição, pelo que não é necessário especificá-la; para personalizar, use um parâmetro aninhado como --dataset.rgb_encoder.vcodec=h264.

Os dados são guardados em ./datasets/bi_so101_task/, com esta estrutura:

```Bash
├── meta/
│   ├── info.json         # Informação do conjunto de dados (fps, formas das características, etc.)
│   ├── episodes/         # Metadados por episódio (chunk-000/...)
│   ├── stats.json        # Estatísticas de normalização de cada característica
│   └── tasks.parquet     # Texto da tarefa → task_index
├── data/                 # Dados de características por fotograma (chunk-*.parquet)
└── videos/               # Um subdiretório por câmara (chunk-*.mp4)
```

### 3.2 Enviar para o Hugging Face Hub

Se quiser o envio automático, mantenha HF_USER e remova root e push_to_hub=false (o envio é o comportamento predefinido). Mantenha as portas e os índices das câmaras consistentes com a tabela de cablagem:

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

> O nome do repositório do Hub carregado é ${HF_USER}/bi_so101_task, correspondendo ao repo_id usado para o treino baseado no Hub em 4.2 abaixo. É primeiro guardada uma cópia local em ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/.

### 3.3 Continuar a gravação (retomar)

Se a gravação terminou inesperadamente (por exemplo, saiu com um clique com o botão direito enquanto estava na fase de reposição), ou se quiser concluir a recolha ao longo de várias sessões, use --resume para continuar a acrescentar episódios ao mesmo conjunto de dados.

**Nota**:

- Tem de adicionar --resume=true, caso contrário o LeRobotDataset.create() dá erro porque o diretório já existe.
- No comando de retomada, --dataset.root e --dataset.repo_id têm de corresponder exatamente à primeira gravação (3.1) (a retomada exige um root explícito).
- --dataset.num_episodes é **quantos episódios gravar desta vez**, não o total pretendido. Por exemplo, se já gravou 15 e quer 50 no total, escreva 35.
- Ao sair, tente sair durante a gravação de um episódio ou logo após este terminar naturalmente; evite sair durante a fase "Reset the environment" (provoca a falha de gravação de um episódio vazio).

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

### 3.4 Reprodução e eliminação de episódios

#### Reproduzir um episódio específico

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

> episode é um índice com base 0, pelo que 24 significa o 25.º episódio.

#### Eliminar um episódio específico

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

Eliminar reescreve o conjunto de dados no local, e os dados originais são copiados para ./datasets/bi_so101_task_old/. Depois de confirmar que o novo conjunto de dados está correto, pode remover manualmente a cópia de segurança:

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### Eliminar o conjunto de dados inteiro

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## Treino com ACT

### 4.1 Treinar a partir de um conjunto de dados local

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

> --dataset.root aponta para o diretório do conjunto de dados gravado em 3.1 (o repo_id tem de corresponder ao usado na gravação). Se o diretório --output_dir já existir, dá imediatamente FileExistsError — use um novo diretório de saída ou adicione --resume=true para continuar o treino.

### 4.2 Treinar a partir do Hugging Face Hub

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

> O comando acima usa os parâmetros predefinidos do ACT (chunk_size=100, dim_model=512, etc.).
> 
> O repo_id tem de corresponder ao nome do repositório usado no envio em 3.2 (o 3.2 adiciona --dataset.no_stamp=true, pelo que o nome do repositório fica fixo como \${HF_USER}/bi_so101_task). Não é necessário --dataset.root para o treino; é descarregado automaticamente do Hub.

## Implementação no Robô Real

> Nota: o lerobot-record serve apenas para recolher dados de demonstração. Use o lerobot-rollout para implementar uma política treinada — a versão atual do lerobot-record já não aceita --policy.path e também rejeita nomes de conjunto de dados com o prefixo eval\.

### 5.1 Avaliação no local (sem gravação de dados)

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

- --duration é o número de segundos de execução; 0 significa sem limite de tempo.
- Para assumir o controlo/parar a meio da execução, adicione --interactive=true e use comandos como /stop e /reset no terminal.

### 5.2 Avaliar e gravar dados (localmente)

Use a estratégia episodic (comporta-se como o antigo lerobot-record: grava por episódio com uma fase de reposição):

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

> Um nome de conjunto de dados de implementação tem de começar por rollout\_ (um requisito rígido da versão atual). Ao gravar localmente, adicione --dataset.root e --dataset.no_stamp=true para evitar que seja anexado um timestamp ao nome do diretório.

### 5.3 Enviar os dados de avaliação para o Hugging Face Hub

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

## Perguntas frequentes

| Problema | Causa | Solução |
|-|-|-|
| A teleoperação pede para recalibrar | O bi_so_follower não encontra ficheiros de calibração com o sufixo \_left / \_right | Recalibre com IDs que incluam _left / \_right, ou copie os ficheiros de calibração existentes |
| O braço leader não se consegue arrastar | O binário do Leader não está desativado | Recalibre ou verifique o motor |
| A retomada da gravação indica que o diretório já existe | Não foi adicionado --resume=true | Adicione --resume=true ao comando lerobot-record |
| --resume=true dá erro e exige um root | A retomada exige um diretório de conjunto de dados explícito | Adicione --dataset.root=./datasets/bi_so101_task ao comando de retomada, correspondendo à primeira gravação |
| O nome do diretório do conjunto de dados tem um timestamp extra, pelo que a reprodução/treino não o encontram | não foi definido no_stamp ao gravar, pelo que foi anexado um timestamp ao repo_id | Adicione --dataset.no_stamp=true ao gravar/retomar |
| --dataset.vcodec=... indica que o parâmetro não existe | É um parâmetro antigo; o parâmetro de codificação de vídeo é agora aninhado | Use --dataset.rgb_encoder.vcodec=h264 em alternativa (o valor predefinido já é libsvtav1) |
| Durante a implementação, o lerobot-record reporta um erro --policy.path / eval\_ | A versão atual do lerobot-record já não inclui a implementação de políticas | Use lerobot-rollout --strategy.type=episodic para a implementação, com nomes de conjunto de dados a começar por rollout_ |
| Os braços esquerdo e direito estão trocados | Configuração de portas errada | Troque left_arm_config.port e right_arm_config.port |
| O treino não encontra o conjunto de dados | Não foi especificado um root para o conjunto de dados local | Adicione --dataset.root=./datasets/xxx ao treinar |
| O conjunto de dados é carregado automaticamente | Não foi definido push_to_hub=false | Adicione --dataset.push_to_hub=false ao gravar |
| Ao sair, reporta You must add one or several frames before calling add_episode | Saiu durante a fase de reposição, pelo que o episódio atual não tem fotogramas | Não afeta os dados já gravados; use --resume=true para continuar a recolha |
