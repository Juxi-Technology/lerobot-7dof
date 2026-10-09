[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | Português (BR) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# Tutorial do braço duplo SO-ARM101

## Introdução

Este guia percorre o fluxo de trabalho completo para treinar um sistema de robô SO-ARM de braço duplo com o LeRobot, incluindo a fiação do hardware, a calibração do braço duplo, a teleoperação de braço duplo, a gravação e o gerenciamento de datasets, o treinamento da policy ACT e a implantação no robô real. Seguindo este guia, você pode usar dois braços leader e dois braços follower para coletar dados de demonstração, treinar uma policy de aprendizado por imitação e executá-la nos braços reais.

Primeiro, faça a fiação de tudo da seguinte forma

| Função | Porta |
|-|-|
| Follower esquerdo | /dev/ttyACM0 |
| Follower direito | /dev/ttyACM1 |
| Leader esquerdo | /dev/ttyACM2 |
| Leader direito | /dev/ttyACM3 |

O tipo follower é so101_follower e o tipo leader é so101_leader (no LeRobot, so100_leader e so101_leader compartilham a mesma implementação).

## Pré-requisitos

### 0.1 Instalar as dependências

Para a configuração do ambiente, consulte o tutorial do SO-ARM:

### 0.2 Permissões de USB

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## Calibração (etapa crítica)

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

Após a calibração, os arquivos são salvos em:

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> Observação sobre os nomes dos diretórios: so101_follower e so100_follower, assim como so101_leader e so100_leader, compartilham a mesma implementação, então os diretórios são unificados como so_follower / so_leader. O leader é um teleoperador, então seus arquivos de calibração ficam em teleoperators/ e não em robots/.

### (Opcional) Se você calibrou antes com outros IDs

Por exemplo, se você usou antes my_awesome_follower_arm1, my_awesome_follower_arm2, etc., você pode copiar os arquivos de calibração:

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

### 2.1 Sem câmeras

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

### 2.2 Com câmeras

Você pode usar lerobot-find-cameras opencv para verificar os índices das câmeras e adicionar ou remover câmeras como quiser.

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

- Fique atento ao ambiente e evite colisões entre os braços follower.

## Gravando um dataset

### 3.1 Salvar localmente (sem enviar para o Hub)

Adicione --dataset.root (o diretório em que os dados são gravados) e --dataset.push_to_hub=false, e adicione --dataset.no_stamp=true para manter o nome do dataset estável (caso contrário, um carimbo de tempo é anexado automaticamente ao repo_id, e depois o resume/reprodução/treinamento não o encontrarão).

> Observação: o repo_id deve conter / (no formato usuário/nome-do-dataset); um dataset local não é realmente enviado.

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

> A codificação de vídeo já é libsvtav1 por padrão, então não é preciso especificá-la; para personalizar, use um parâmetro aninhado como --dataset.rgb_encoder.vcodec=h264.

Os dados são salvos em ./datasets/bi_so101_task/, com esta estrutura:

```Bash
├── meta/
│   ├── info.json         # Informações do dataset (fps, formatos dos recursos, etc.)
│   ├── episodes/         # Metadados de cada episódio (chunk-000/...)
│   ├── stats.json        # Estatísticas de normalização de cada recurso
│   └── tasks.parquet     # Texto da tarefa → task_index
├── data/                 # Dados dos recursos por quadro (chunk-*.parquet)
└── videos/               # Um subdiretório por câmera (chunk-*.mp4)
```

### 3.2 Enviar para o Hugging Face Hub

Se você quiser o envio automático, mantenha HF_USER e remova root e push_to_hub=false (o envio é o padrão). Mantenha as portas e os índices das câmeras consistentes com a tabela de fiação:

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

> O nome do repositório no Hub enviado é ${HF_USER}/bi_so101_task, correspondendo ao repo_id usado para o treinamento baseado no Hub no item 4.2 abaixo. Uma cópia local é primeiro salva em ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/.

### 3.3 Continuar a gravação (resume)

Se a gravação saiu inesperadamente (por exemplo, você saiu com um clique com o botão direito durante a fase de reset), ou se você quiser concluir a coleta em várias sessões, use --resume para continuar acrescentando episódios ao mesmo dataset.

**Observação**:

- Você deve adicionar --resume=true, caso contrário LeRobotDataset.create() dá erro porque o diretório já existe.
- No comando de resume, --dataset.root e --dataset.repo_id devem corresponder exatamente à primeira gravação (3.1) (o resume exige um root explícito).
- --dataset.num_episodes é **quantos episódios gravar desta vez**, e não o total desejado. Por exemplo, se você já gravou 15 e quer 50 no total, escreva 35.
- Ao sair, tente encerrar durante a gravação de um episódio ou logo depois que ele terminar naturalmente; evite sair durante a fase "Reset the environment" (isso faz com que um episódio vazio não consiga ser salvo).

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

### 3.4 Reprodução e exclusão de episódios

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

> episode é um índice com base 0, então 24 significa o 25º episódio.

#### Excluir um episódio específico

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

A exclusão reescreve o dataset no lugar, e os dados originais são copiados como backup para ./datasets/bi_so101_task_old/. Depois de confirmar que o novo dataset está correto, você pode remover manualmente o backup:

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### Excluir o dataset inteiro

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## Treinamento com ACT

### 4.1 Treinar a partir de um dataset local

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

> --dataset.root aponta para o diretório do dataset gravado em 3.1 (o repo_id deve corresponder ao usado na gravação). Se o diretório --output_dir já existir, ele gera FileExistsError imediatamente — use um novo diretório de saída ou adicione --resume=true para continuar o treinamento.

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

> O comando acima usa os parâmetros padrão do ACT (chunk_size=100, dim_model=512, etc.).
> 
> O repo_id deve corresponder ao nome do repositório usado no envio em 3.2 (o 3.2 adiciona --dataset.no_stamp=true, então o nome do repositório fica fixo como \${HF_USER}/bi_so101_task). Não é preciso --dataset.root para o treinamento; ele é baixado do Hub automaticamente.

## Implantação no robô real

> Observação: o lerobot-record serve apenas para coletar dados de demonstração. Use o lerobot-rollout para implantar uma policy treinada — a versão atual do lerobot-record não aceita mais --policy.path e também rejeita nomes de dataset com o prefixo eval\.

### 5.1 Avaliação no local (sem gravar dados)

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
- Para assumir o controle/parar no meio da execução, adicione --interactive=true e use comandos como /stop e /reset no terminal.

### 5.2 Avaliar e gravar dados (localmente)

Use a estratégia episodic (comporta-se como o antigo lerobot-record: grava por episódio com uma fase de reset):

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

> Um nome de dataset de implantação deve começar com rollout\_ (requisito obrigatório da versão atual). Ao gravar localmente, adicione --dataset.root e --dataset.no_stamp=true para evitar que um carimbo de tempo seja anexado ao nome do diretório.

### 5.3 Enviar dados de avaliação para o Hugging Face Hub

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
| A teleoperação pede para recalibrar | O bi_so_follower não encontra arquivos de calibração com o sufixo \_left / \_right | Recalibre com IDs que incluam _left / \_right, ou copie arquivos de calibração existentes |
| Não é possível arrastar o braço leader | O torque do leader não foi desabilitado | Recalibre ou verifique o motor |
| A continuação da gravação reporta que o diretório já existe | --resume=true não foi adicionado | Adicione --resume=true ao comando lerobot-record |
| --resume=true dá erro e exige um root | O resume exige um diretório de dataset explícito | Adicione --dataset.root=./datasets/bi_so101_task ao comando de resume, correspondendo à primeira gravação |
| O nome do diretório do dataset tem um carimbo de tempo extra, então a reprodução/treinamento não o encontra | no_stamp não foi definido na gravação, então um carimbo de tempo foi anexado ao repo_id | Adicione --dataset.no_stamp=true ao gravar/continuar |
| --dataset.vcodec=... reporta que o parâmetro não existe | É um parâmetro antigo; o parâmetro de codificação de vídeo agora é aninhado | Use --dataset.rgb_encoder.vcodec=h264 no lugar (o padrão já é libsvtav1) |
| Durante a implantação, o lerobot-record reporta um erro de --policy.path / eval\_ | A versão atual do lerobot-record não inclui mais a implantação de policy | Use lerobot-rollout --strategy.type=episodic para a implantação, com nomes de dataset começando com rollout_ |
| Os braços esquerdo e direito estão trocados | Configuração de porta errada | Troque left_arm_config.port e right_arm_config.port |
| O treinamento não encontra o dataset | Nenhum root foi especificado para o dataset local | Adicione --dataset.root=./datasets/xxx ao treinar |
| O dataset é enviado automaticamente | push_to_hub=false não foi definido | Adicione --dataset.push_to_hub=false ao gravar |
| Na saída, reporta You must add one or several frames before calling add_episode | Você saiu durante a fase de reset, então o episódio atual não tem quadros | Não afeta os dados já gravados; use --resume=true para continuar a coleta |
