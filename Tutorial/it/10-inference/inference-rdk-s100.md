[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | [Français](../fr/10-inference/inference-rdk-s100.md) | Italiano | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# Inferenza su D-Robotics RDK S100

Per il flusso di implementazione dettagliato, fai riferimento a questo link<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="LeRobot ACT Policy Full Workflow Document" type="doc"></cite>



## Distribuzione end-to-end del modello ACT su RDK S100/S100P

Questa sezione ti guida attraverso l'intero ciclo di distribuzione del modello ACT sull'hardware della serie D-Robotics RDK S100. L'intero processo prevede tre fasi principali: **esportazione del modello**, **compilazione di quantizzazione** e **esecuzione a bordo**.

<callout emoji="💡">
**Prerequisiti:**
- **Macchina di sviluppo (Host):** usata per eseguire i passaggi 1 e 2, di solito è la macchina su cui addestri i modelli (deve avere prestazioni adeguate e Docker installato).
- **Scheda (Edge):** la D-Robotics RDK S100/S100P, usata per eseguire il passaggio 3.
- **Toolchain:** questo articolo si basa sul repository `rdk_LeRobot_tools`; vedi il [repository GitHub](https://github.com/D-Robotics/rdk_LeRobot_tools) per i dettagli.
</callout>

<callout emoji="🚨">
**Nota importante sulla compatibilità delle versioni (da leggere assolutamente):** l'attuale flusso di esportazione ONNX di `rdk_LeRobot_tools` è pienamente compatibile con i **dataset LeRobot v2.1**. Poiché la versione più recente v3.0 modifica la struttura dei dati, si **consiglia vivamente**, prima di svolgere le operazioni di questa sezione, di portare il repository principale `lerobot` allo specifico commit compatibile con la v2.1, in modo che il flusso di esportazione funzioni senza intoppi.
*ID commit consigliato:* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### Fase 1: esportare il modello in formato ONNX 💻 (sulla macchina di sviluppo)

Innanzitutto, dobbiamo esportare il modello **addestrato con PyTorch** in un formato intermedio (ONNX).



#### **1. Clonare il repository della toolchain** 

Vai nella tua directory di lavoro `lerobot` e clona la toolchain specifica per RDK:

```Bash
cd lerobot

# 1. Passa alla versione stabile compatibile con i dataset v2.1
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. Clona la toolchain specifica per D-Robotics RDK
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. Configurare i parametri di esportazione** 

Modifica il file `rdk_LeRobot_tools/bpu_export_config.yaml` e adatta la configurazione ai tuoi percorsi effettivi:

```YAML
dataset:
  root: "data/so101_pick_place" # percorso assoluto o relativo al tuo dataset
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # percorso ai pesi originali del modello PyTorch
type: "nash-e" # architettura hardware di destinazione; RDK S100 corrisponde a nash-e / S100P corrisponde a nash-m
```



#### 3. Eseguire lo script di esportazione

```Bash
# Esporta ONNX (macchina di sviluppo)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **Indicatore di successo**: nella directory corrente viene creata una cartella `bpu_export_output`, che contiene lo script `build_all.sh` e i dati di calibrazione per la quantizzazione necessari in seguito.



### Fase 2: compilare il modello BPU 🐳 (in un ambiente Docker sulla macchina di sviluppo)

La quantizzazione e la compilazione dei modelli BPU di D-Robotics richiedono un ambiente OpenExplorer (OE). Consigliamo di usare Docker per isolare l'ambiente.



#### **1.** **Preparare l'ambiente Docker e l'immagine** 

Assicurati che Docker sia installato sulla macchina di sviluppo ([guida ufficiale all'installazione](https://docs.docker.com/engine/install/)). Scarica l'immagine CPU consigliata e caricala:

```Bash
# Carica l'archivio dell'immagine offline scaricato
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. Avviare il container di compilazione**

<callout emoji="⚠️">
**Avvertenza sugli errori comuni**: la compilazione del modello richiede una grande quantità di memoria condivisa. Assicurati di aggiungere l'argomento `--shm-size=15g`, altrimenti sono molto probabili errori di memoria IPC.
</callout>

Monta nella directory del container la directory di lavoro della macchina di sviluppo (che contiene la cartella appena esportata):

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(Nota: sostituisci `<docker-image-name>` con il nome effettivo dell'immagine che vedi con `sudo docker images`.)



#### **3.** **Eseguire la compilazione all'interno del container** 

Una volta dentro il container, esegui lo script di compilazione con un solo clic:

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **Verificare gli artefatti di build** 

Al termine della compilazione, sotto `bpu_export_output` viene creata una cartella `bpu_output/`. Contiene tutti i file principali necessari per l'esecuzione sulla scheda RDK: 

- Clicca per visualizzare la struttura della directory `bpu_output/`

  - `BPU_ACTPolicy_TransformerLayers.hbm` (file del modello quantizzato)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (file del modello quantizzato)
  - `action_mean.npy` e diversi altri parametri di normalizzazione del dataset
  - `camera1_mean.npy` e altri parametri statistici della telecamera

---

### Fase 3: distribuzione e inferenza a bordo 🤖 (sulla RDK S100)

<callout emoji="📌">
**Verifica dei prerequisiti:**
1. La scheda RDK ha già l'ambiente di runtime `D-Robotics/lerobot` configurato, con `hbm_runtime` installato.
2. L'intera cartella `bpu_output/` generata nel passaggio precedente è stata copiata completamente sulla scheda RDK, tramite `scp`, una chiavetta USB o simili.
3. La configurazione di base della teleoperazione è già stata eseguita, assicurando che la porta seriale del braccio, la porta USB della telecamera e il file di calibrazione siano configurati correttamente.
</callout>



#### **1.** **Eseguire l'inferenza accelerata da BPU**

Nel terminale della scheda RDK, vai nella directory della toolchain e avvia lo script di controllo:

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ Risoluzione dei problemi

Se incontri problemi durante una distribuzione reale, confrontali con il seguente elenco:

- **Il braccio non si muove?**

  - Controlla che il dispositivo sia montato: digita `ls /dev/ttyACM*` nel terminale e conferma che la porta seriale del braccio sia corretta.
  - Controlla i permessi: prova a eseguire lo script di inferenza con `sudo`, oppure aggiungi l'utente corrente al gruppo `dialout`.
- **Errore di streaming della telecamera / immagine anomala / il braccio trema sul posto?**

  - Verifica se l'indice della telecamera è cambiato a causa di un hot-plug e controlla che i parametri della telecamera nel codice corrispondano all'effettivo `/dev/video*`.
- **La copia dei file generati dal container sulla macchina di sviluppo segnala "permessi insufficienti"?**

  - I file creati in una directory montata da Docker appartengono a root per impostazione predefinita; esegui `sudo chown -R $USER:$USER bpu_export_output` sulla macchina di sviluppo per risolvere.
