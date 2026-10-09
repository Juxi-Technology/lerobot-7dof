[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | Italiano | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | [한국어](../ko/07-collect-dataset-real/create-huggingface-account.md) | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Registrare un account Hugging Face (opzionale)

## Configurare un mirror HuggingFace basato in Cina

- Ubuntu

```Shell
sudo nano ~/.bashrc

# Aggiungi questo in fondo al file
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# Output
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# Aggiungi questo in fondo al file
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# Output
# https://hf-mirror.com
```



## Creare un token

https://huggingface.co/settings/tokens

![L'immagine mostra la pagina "Impostazioni token" del sito Hugging Face. Sul lato sinistro della pagina ci sono l'avatar dell'utente e le informazioni personali, a destra c'è l'area "User Access Tokens", che spiega che i token di accesso servono per l'autenticazione e permettono alle applicazioni di eseguire operazioni in base ai permessi del token, sottolineando di non condividerli con altri. In alto a destra c'è il pulsante "+ Create new token", usato per creare un nuovo token. Questa immagine è collegata alla sezione "Creare un token" del documento e mostra visivamente il punto di accesso per creare un token.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![Questa immagine mostra la pagina di creazione di un nuovo token di accesso sulla piattaforma Hugging Face. Nell'area "Token type" è evidenziata l'opzione "Write", nel campo "Token name" è già inserito il valore "lerobot_zihao_token", in fondo alla pagina c'è il pulsante "Create token" per inviare l'operazione di creazione e sul lato sinistro della pagina sono mostrate le informazioni dell'account attualmente connesso.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![L'immagine mostra la finestra "Save your Access Token" del sito Hugging Face. La finestra avvisa l'utente di conservare il valore del token in un luogo sicuro, perché dopo la chiusura non sarà più possibile visualizzarlo. Sotto viene mostrato il valore del token "<your-huggingface-token>", con un pulsante "Copy" per copiarlo. Sotto compaiono anche il nome del token "lerobot_zihao_token" e il permesso "WRITE". Questa immagine è collegata al passaggio "Creare un token" del documento e serve a guidare l'utente nella registrazione del token generato.](../../en/images/d24-03.png)
</column>
</grid>

## Annotare il token

Ad esempio, il mio è:

```Shell
<your-huggingface-token>
```

## Collegare il token

```Shell
hf auth login

hf auth whoami
```

![L'immagine mostra l'interfaccia per gestire un account Hugging Face da riga di comando su un sistema Mac. Prima viene eseguito il comando "hf auth login", che segnala che esiste già un token e suggerisce di usare "hf auth whoami" per ottenere le informazioni oppure "hf auth logout" per disconnettersi. Poi viene inserito il token e, dopo la conferma, viene mostrato che il token è stato salvato nel percorso specificato e che il token attivo è "lerobot zihao token"; infine viene eseguito il comando "hf auth whoami", che mostra l'utente "TommyZihao". Questa immagine corrisponde alla sezione "Creare un token" del documento e mostra visivamente le operazioni e il risultato della creazione del token.](../../en/images/d24-04.png)

## Creare un repository di dataset

<grid>
<column width-ratio="0.480604">
![Questa è una schermata della piattaforma Hugging Face; in alto a sinistra della pagina ci sono le voci Community, Docs, Pricing e altre, mentre le informazioni dell'account personale TommyZihao si trovano nell'area centrale dell'interfaccia; quello evidenziato dal riquadro verde è l'icona dell'avatar dell'utente nell'angolo in alto a destra. Nel menu centrale dell'interfaccia, l'opzione "+ New Dataset" è evidenziata da un riquadro rosso; questa opzione corrisponde al passaggio "Creare un repository di dataset" del contesto e, facendo clic su di essa, è possibile iniziare a creare il repository di dataset.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![L'immagine mostra l'interfaccia per creare un nuovo repository di dataset su Hugging Face. Nel campo "Dataset name" è inserito "lerobot_zihao_dataset_a", "License" è impostata su "mit" e l'opzione "Public" è selezionata. In basso c'è il pulsante "Create dataset". Questa immagine corrisponde alla sezione "Creare un repository di dataset" del documento e mostra visivamente le informazioni chiave da inserire durante la creazione del repository di dataset, aiutando l'utente a comprendere l'interfaccia e i campi richiesti per creare un repository di dataset su Hugging Face.](../../en/images/d24-06.png)
</column>
</grid>

![Questa è l'interfaccia web della piattaforma Hugging Face, che mostra il contenuto del repository di dataset chiamato lexobot_zihao_dataset_a creato dall'utente TommyZhao, in tema Dark. Sul lato sinistro della pagina c'è l'opzione di modifica "Edit Dataset Card" e sono inoltre offerti diversi modi per inviare i file del dataset, con esempi di comandi per CLI, Python, HTTPS e SSH; è inoltre indicato che la License di questo dataset è mit.](../../en/images/d24-07.png)
