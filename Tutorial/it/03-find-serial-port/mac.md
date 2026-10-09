[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | Italiano | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Computer Mac

## Controllare la porta

```Shell
ls /dev/tty.*
```

Il risultato è simile all'immagine seguente; una qualsiasi delle due porte funzionerà

![L'immagine mostra l'output nel terminale del Mac dopo l'esecuzione del comando "ls /dev/tty.*", che elenca diverse porte. Due di esse, "/dev/tty.usbmodem5AAF2194741" e "/dev/tty.wchusbserial5AAF2194741", sono evidenziate con riquadri rossi. Il contesto riguarda il controllo delle porte, e il risultato è simile a questa immagine; è possibile usare una qualsiasi delle due porte. L'immagine presenta le informazioni sulle porte da controllare, è strettamente correlata al contenuto "Controllare la porta" qui sopra ed è una visualizzazione del risultato dell'operazione di controllo delle porte.](../../en/images/d09-01.png)

## Concedere le autorizzazioni alla porta

Concedi a tutti gli utenti il permesso di leggere e scrivere su questi dispositivi seriali

```Shell
chmod 666 /dev/tty.*
```

## Annotare le mie porte

Braccio Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Braccio Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Perché su un Mac ci sono due porte?

La scheda del controller dei servo che utilizziamo è **riconosciuta da macOS come due diversi tipi di driver seriali contemporaneamente**, perciò vengono mostrate due porte:

- Una è il driver seriale generico predefinito del sistema (`/dev/tty.usbmodemxxxx`)
- L'altra è il driver seriale dedicato fornito dal produttore del chip (ad esempio, "wch" qui si riferisce al chip CH340/CH341 di Nanjing Qinheng) (`/dev/tty.wchusbserialxxxx`)

Questo è normale — **le due porte corrispondono in realtà allo stesso dispositivo hardware**, e puoi collegarti e comunicare attraverso una qualsiasi delle due (ad esempio, scegli semplicemente una delle due porte nel software che controlla il braccio robotico).

Se in seguito incontri un errore usando una porta, prova a passare all'altra.
