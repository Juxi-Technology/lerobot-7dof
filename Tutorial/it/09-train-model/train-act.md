[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | Italiano | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# Riga di comando di addestramento - ACT (consigliato per i principianti)

## Documentazione di riferimento

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## Perché iniziare con l'algoritmo ACT

ACT è il primo modello che si consiglia di addestrare quando si inizia con LeRobot. I suoi vantaggi sono:

- Il modello è molto leggero, con soli 80 milioni di parametri addestrabili
- L'addestramento converge rapidamente e anche l'inferenza è veloce
- Si vedono risultati dopo appena un'ora di addestramento su una singola GPU
- L'archivio del modello ACT è di circa 200 MB, quindi è facile da archiviare e trasferire
- Di solito bastano circa 30 episodi di dati
- Può essere distribuito per l'inferenza su un host Ubuntu, un Mac, un PC Windows, persino un Raspberry Pi
- L'inferenza su un robot reale funziona piuttosto bene ed è più che sufficiente per compiti semplici come prelievo, stretta di mano e posizionamento della penna
- L'algoritmo ACT è già integrato nell'ambiente base di LeRobot, quindi non servono librerie aggiuntive

## Riga di comando

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## Note sulla riga di comando

Un `\` di continuazione di riga può avere solo uno spazio prima e nessuno spazio dopo

I parametri mostrati in rosso devono essere verificati o modificati prima di ogni esecuzione

| Parametro della riga di comando | Descrizione |
|-|-|
| --dataset.repo_id | Repo_ID del dataset su HuggingFace |
| --dataset.root | Percorso locale del dataset |
| --dataset.revision | Versione del dataset, specificata quando hai caricato il dataset su HuggingFace |
| --dataset.streaming | Il dataset è locale, quindi deve essere `false`, poiché i dati sono già su disco e non serve una lettura in streaming |
| --dataset.split | Ha come valore predefinito `train`, il che significa che l'intero dataset viene usato come insieme di addestramento |
| --policy.type | L'algoritmo da addestrare, come act, smolvla, diffusion, pi0, wallx |
| --output_dir | Directory in cui viene salvata la struttura di output |
| --job_name | Nome di questo job di addestramento |
| --policy.device | Dispositivo di calcolo |
| --wandb.enable | Abilita la visualizzazione su wandb |
| --wandb.project | Nome del progetto wandb |
| --policy.push_to_hub | Carica il modello addestrato su HuggingFace |
| --steps | Numero di step di addestramento |
| --batch_size | Quantità di dati fornita per step; riducila se esaurisci la memoria della GPU |
|  |  |

## Processo di addestramento

<grid>
<column width-ratio="0.357753">
![Questa immagine mostra un esempio d'uso del comando di addestramento `lerobot-train` dalla riga di comando. Il comando imposta diversi parametri come `--dataset.repo_id` e `--dataset.root` per specificare i dettagli del dataset, imposta `--policy.type` su `act` e `--output_dir` sulla directory di output `outputs/lerobot_train/output_a`, insieme ad altri parametri come `--job_name` e `--policy.device`. Sono inoltre elencati i valori predefiniti di parametri come `--dataset.split` e `--policy.push_to_hub`. L'immagine è strettamente legata al contesto e mostra visivamente come vengono impostati i parametri del comando di addestramento.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![Questa immagine mostra l'output della riga di comando durante l'addestramento. Visualizza dettagli dell'addestramento del modello come le impostazioni di scheduler, steps e use_policy_training_preset, insieme a parametri relativi al dataset. Mostra inoltre informazioni come il numero di parametri del modello e la loss, ad esempio num_total_params pari a 55917096 (52M) e una loss di 0.626. Sotto compaiono informazioni sul download dei file, come il download di "https://download.pytorch.org/models/resnet18-f37072fd.pth" nella directory /home/featurize/.cache/torch/hub/checkpoints. L'immagine è collegata alla riga di comando di addestramento descritta nel contesto e mostra visivamente ciò che la riga di comando restituisce durante l'addestramento.](../../en/images/d36-02.png)
</column>
</grid>

![Questa immagine mostra le informazioni di log prodotte durante l'addestramento. Il log registra diversi step di addestramento in ordine cronologico, includendo l'orario, il conteggio delle iterazioni, la loss e il learning rate — ad esempio, alle 15:11:53 del 14 gennaio 2024 il conteggio delle iterazioni era 131k e la loss era 0.368. Qui `INFO` è il tipo di log, `train` la fase di addestramento, `step` il conteggio delle iterazioni, `loss` il valore di loss e `lr` il learning rate. L'immagine è collegata alla sezione delle note sulla riga di comando del documento e presenta visivamente i dati chiave dell'esecuzione dell'addestramento.](../../en/images/d36-03.png)

L'archivio del modello è di circa 300 MB
