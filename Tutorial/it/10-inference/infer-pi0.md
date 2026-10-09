[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | Italiano | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# Riga di comando di inferenza - pi0

## Ubuntu

- Elimina il dataset esistente con prefisso eval (se presente)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- Riga di comando di inferenza

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![Questa immagine mostra un errore che compare quando ci si connette alla macchina via SSH in un ambiente Ubuntu. Mostra un errore della console che indica che la piattaforma non è supportata, che non è possibile stabilire la connessione X e che occorre assicurarsi che un server X sia in esecuzione e che la variabile d'ambiente DISPLAY sia impostata correttamente. Mostra inoltre un avviso relativo a un ambiente headless e la registrazione dell'episodio 0. L'immagine è collegata alla riga di comando di inferenza su Ubuntu e potrebbe rappresentare una situazione anomala incontrata durante l'esecuzione.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![Questa immagine mostra l'output durante l'esecuzione della riga di comando di inferenza in un ambiente Ubuntu. Durante l'esecuzione compaiono più volte messaggi di errore "E0119", che indicano che non esiste una config triton valida durante l'autotuning e che le risorse sono esaurite, ad esempio memoria condivisa insufficiente. Mostra inoltre i parametri di esecuzione di diversi modelli triton_mm, come ALLOW_TF32, BLOCK_K e BLOCK_M, insieme ai valori corrispondenti di ACC_TYPE, ALLOW_TF32, BLOCK_K e BLOCK_M. L'immagine è collegata alla riga di comando di inferenza su Ubuntu e mostra una carenza di risorse incontrata durante l'esecuzione.](../../en/images/d51-02.png)
</column>
</grid>

![Questa immagine mostra il terminale durante una sessione di inferenza da riga di comando in un ambiente Ubuntu. Mostra i risultati di diverse istruzioni triton_mm, ad esempio triton_mm_3644 che impiega 0.2355 ms, tutte con il tipo t1.float32 e ALLOW_TF32=True, e mostra anche parametri come BLOCK_K. Alla fine mostra il benchmarking SingleProcess AUTOTUNE che impiega 0.7305 secondi e 0.0001 secondi per precompilare 20 scelte. L'immagine è collegata alla riga di comando di inferenza su Ubuntu e mostra l'esecuzione effettiva.](../../en/images/d51-03.png)

> **Video in sospeso**: il testo originale incorpora qui `VID_20260120_182109.mp4` (originariamente 310 MB). Dal lato Feishu non è stato fornito alcuno stream video scaricabile per questo file, solo i metadati, quindi non è stato possibile acquisirlo. Per visualizzarlo, vedi il [documento originale](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg).



## Mac

- Elimina il dataset esistente con prefisso eval (se presente)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Riga di comando di inferenza

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![Questa immagine mostra il terminale durante una sessione di inferenza da riga di comando (11 - yolo26) in un ambiente Ubuntu. Mostra le informazioni sulla versione di Python 3.12 e la registrazione di robot-type impostato su follower. Elenca inoltre parametri relativi alla telecamera come color_mode, fourcc, fps, height e width, e mostra il percorso da cui viene caricato il modello insieme ad alcuni messaggi di avviso, come errori di caricamento del modello. L'immagine è collegata alla riga di comando di inferenza su Ubuntu e presenta il feedback del terminale durante l'operazione.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![Questa immagine mostra l'output della riga di comando durante l'esecuzione dell'inferenza con codice Python in un ambiente Ubuntu. Contiene diverse informazioni, come il caricamento riuscito del "PIBPytorch model", "WARNING" su chiavi del modello che potrebbero dover essere gestite, e "INFO" che indica che la telecamera OpenCV si è connessa correttamente. Mostra inoltre più volte l'avviso "huggingface/tokenizers: The process current just got forked...", segnalando un problema di parallelismo causato dal fork. L'immagine è collegata alla riga di comando di inferenza su Ubuntu descritta nel contesto e mostra i vari messaggi e avvisi che possono comparire in fase di esecuzione.](../../en/images/d51-05.png)
</column>
</grid>

## Perché l'inferenza su Mac fa scattare il braccio

- Il dataset è troppo piccolo
- La GPU non ha memoria sufficiente; serve una scheda della serie 50
