[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | [Français](../fr/so-arm101-dual-arm.md) | Italiano | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# Tutorial del SO-ARM101 a doppio braccio

## Introduzione

Questa guida illustra il flusso di lavoro completo per addestrare un sistema robotico SO-ARM a doppio braccio con LeRobot, inclusi il cablaggio hardware, la calibrazione dei due bracci, la teleoperazione a doppio braccio, la registrazione e la gestione dei dataset, l'addestramento della policy ACT e il deployment sul robot reale. Seguendo questa guida, puoi usare due bracci leader e due bracci follower per raccogliere dati di dimostrazione, addestrare una policy di imitation learning ed eseguirla sui bracci reali.

Per prima cosa, collega tutto come segue

| Ruolo | Porta |
|-|-|
| Follower sinistro | /dev/ttyACM0 |
| Follower destro | /dev/ttyACM1 |
| Leader sinistro | /dev/ttyACM2 |
| Leader destro | /dev/ttyACM3 |

Il tipo follower è so101_follower e il tipo leader è so101_leader (in LeRobot, so100_leader e so101_leader condividono la stessa implementazione).

## Prerequisiti

### 0.1 Installare le dipendenze

Per la configurazione dell'ambiente, fai riferimento al tutorial SO-ARM:

### 0.2 Permessi USB

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## Calibrazione (passaggio critico)

### 1.1 Calibrare il follower sinistro

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 Calibrare il follower destro

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 Calibrare il leader sinistro

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 Calibrare il leader destro

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

Dopo la calibrazione, i file vengono salvati in:

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> Nota sui nomi delle directory: so101_follower e so100_follower, così come so101_leader e so100_leader, condividono la stessa implementazione, quindi le directory vengono unificate come so_follower / so_leader. Il leader è un teleoperatore, quindi i suoi file di calibrazione si trovano sotto teleoperators/ anziché robots/.

### (Opzionale) Se in precedenza hai calibrato con altri ID

Ad esempio, se in precedenza hai usato my_awesome_follower_arm1, my_awesome_follower_arm2, ecc., puoi copiare i file di calibrazione:

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

## Teleoperazione a doppio braccio

### 2.1 Senza telecamere

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

### 2.2 Con le telecamere

Puoi usare lerobot-find-cameras opencv per controllare gli indici delle telecamere e aggiungere o rimuovere telecamere come preferisci.

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

### Suggerimenti di sicurezza

- Tieni d'occhio l'ambiente circostante ed evita le collisioni tra i bracci follower.

## Registrare un dataset

### 3.1 Salvare in locale (senza caricare sull'Hub)

Aggiungi --dataset.root (la directory in cui vengono scritti i dati) e --dataset.push_to_hub=false, e aggiungi --dataset.no_stamp=true per mantenere stabile il nome del dataset (altrimenti al repo_id viene automaticamente aggiunto un timestamp, e successivamente resume/riproduzione/addestramento non lo troveranno).

> Nota: il repo_id deve contenere / (nella forma username/dataset-name); un dataset locale in realtà non viene caricato.

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

> La codifica video è già libsvtav1 per impostazione predefinita, quindi non serve specificarla; per personalizzarla, usa un parametro annidato come --dataset.rgb_encoder.vcodec=h264.

I dati vengono salvati in ./datasets/bi_so101_task/, con questa struttura:

```Bash
├── meta/
│   ├── info.json         # Informazioni sul dataset (fps, forme delle feature, ecc.)
│   ├── episodes/         # Metadati per episodio (chunk-000/...)
│   ├── stats.json        # Statistiche di normalizzazione per ciascuna feature
│   └── tasks.parquet     # Testo del compito → task_index
├── data/                 # Dati delle feature per fotogramma (chunk-*.parquet)
└── videos/               # Una sottodirectory per telecamera (chunk-*.mp4)
```

### 3.2 Caricare su Hugging Face Hub

Se vuoi il caricamento automatico, mantieni HF_USER e rimuovi root e push_to_hub=false (il caricamento è l'impostazione predefinita). Mantieni le porte e gli indici delle telecamere coerenti con la tabella del cablaggio:

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

> Il nome del repository Hub caricato è ${HF_USER}/bi_so101_task, che corrisponde al repo_id usato per l'addestramento basato su Hub al punto 4.2 più sotto. Una copia locale viene prima salvata in ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/.

### 3.3 Continuare la registrazione (resume)

Se la registrazione è terminata in modo imprevisto (ad esempio, hai chiuso con un clic destro durante la fase di reset), oppure vuoi completare la raccolta in più sessioni, usa --resume per continuare ad aggiungere episodi allo stesso dataset.

**Nota**:

- Devi aggiungere --resume=true, altrimenti LeRobotDataset.create() restituisce un errore perché la directory esiste già.
- Nel comando di resume, --dataset.root e --dataset.repo_id devono corrispondere esattamente alla prima registrazione (3.1) (il resume richiede un root esplicito).
- --dataset.num_episodes è **quanti episodi registrare questa volta**, non il totale desiderato. Ad esempio, se hai già registrato 15 episodi e ne vuoi 50 in totale, scrivi 35.
- In fase di uscita, cerca di chiudere durante la registrazione di un episodio o subito dopo che è terminato naturalmente; evita di uscire durante la fase "Reset the environment" (provoca il fallimento del salvataggio di un episodio vuoto).

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

### 3.4 Riproduzione ed eliminazione di episodi

#### Riprodurre un episodio specifico

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

> episode è un indice che parte da 0, quindi 24 significa il 25° episodio.

#### Eliminare un episodio specifico

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

L'eliminazione riscrive il dataset sul posto e i dati originali vengono salvati come backup in ./datasets/bi_so101_task_old/. Una volta confermato che il nuovo dataset è corretto, puoi rimuovere manualmente il backup:

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### Eliminare l'intero dataset

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## Addestramento ACT

### 4.1 Addestrare da un dataset locale

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

> --dataset.root punta alla directory del dataset registrata al punto 3.1 (il repo_id deve corrispondere a quello usato durante la registrazione). Se la directory --output_dir esiste già, viene sollevato immediatamente FileExistsError — usa una nuova directory di output oppure aggiungi --resume=true per continuare l'addestramento.

### 4.2 Addestrare da Hugging Face Hub

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

> Il comando sopra usa i parametri predefiniti di ACT (chunk_size=100, dim_model=512, ecc.).
> 
> Il repo_id deve corrispondere al nome del repository usato durante il caricamento al punto 3.2 (3.2 aggiunge --dataset.no_stamp=true, quindi il nome del repository è fisso come \${HF_USER}/bi_so101_task). Per l'addestramento non serve --dataset.root; viene scaricato automaticamente dall'Hub.

## Deployment sul robot reale

> Nota: lerobot-record serve solo a raccogliere dati di dimostrazione. Usa lerobot-rollout per distribuire una policy addestrata — la versione attuale di lerobot-record non accetta più --policy.path e rifiuta anche i nomi di dataset con il prefisso eval\_.

### 5.1 Valutazione sul campo (nessun dato registrato)

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

- --duration è il numero di secondi di esecuzione; 0 significa nessun limite di tempo.
- Per prendere il controllo/interrompere durante l'esecuzione, aggiungi --interactive=true e usa comandi come /stop e /reset nel terminale.

### 5.2 Valutare e registrare i dati (in locale)

Usa la strategia episodic (si comporta come il vecchio lerobot-record: registra per episodio con una fase di reset):

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

> Un nome di dataset per il deployment deve iniziare con rollout\_ (un requisito inderogabile della versione attuale). Quando registri in locale, aggiungi --dataset.root e --dataset.no_stamp=true per evitare che al nome della directory venga aggiunto un timestamp.

### 5.3 Caricare i dati di valutazione su Hugging Face Hub

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

## Domande frequenti

| Problema | Causa | Soluzione |
|-|-|-|
| La teleoperazione chiede di ricalibrare | bi_so_follower non riesce a trovare i file di calibrazione con il suffisso \_left / \_right | Ricalibra con ID che includono _left / \_right, oppure copia i file di calibrazione esistenti |
| Il braccio leader non si riesce a trascinare | La coppia del leader non è disattivata | Ricalibra o controlla il motore |
| Il resume della registrazione segnala che la directory esiste già | Non è stato aggiunto --resume=true | Aggiungi --resume=true al comando lerobot-record |
| --resume=true restituisce un errore e richiede un root | Il resume richiede una directory del dataset esplicita | Aggiungi --dataset.root=./datasets/bi_so101_task al comando di resume, uguale alla prima registrazione |
| Il nome della directory del dataset ha un timestamp in più, quindi riproduzione/addestramento non riescono a trovarlo | no_stamp non è stato impostato durante la registrazione, quindi al repo_id è stato aggiunto un timestamp | Aggiungi --dataset.no_stamp=true durante la registrazione/il resume |
| --dataset.vcodec=... segnala che il parametro non esiste | È un parametro vecchio; ora il parametro di codifica video è annidato | Usa invece --dataset.rgb_encoder.vcodec=h264 (l'impostazione predefinita è già libsvtav1) |
| Durante il deployment, lerobot-record segnala un errore relativo a --policy.path / eval\_ | La versione attuale di lerobot-record non include più il deployment delle policy | Usa lerobot-rollout --strategy.type=episodic per il deployment, con nomi di dataset che iniziano con rollout_ |
| I bracci sinistro e destro sono invertiti | Configurazione delle porte errata | Scambia left_arm_config.port e right_arm_config.port |
| L'addestramento non riesce a trovare il dataset | Non è stato specificato un root per il dataset locale | Aggiungi --dataset.root=./datasets/xxx durante l'addestramento |
| Il dataset viene caricato automaticamente | push_to_hub=false non è stato impostato | Aggiungi --dataset.push_to_hub=false durante la registrazione |
| All'uscita, segnala You must add one or several frames before calling add_episode | Sei uscito durante la fase di reset, quindi l'episodio corrente non ha fotogrammi | Non influisce sui dati già registrati; usa --resume=true per continuare la raccolta |
