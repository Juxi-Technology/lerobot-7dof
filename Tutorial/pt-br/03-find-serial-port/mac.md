[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | Português (BR) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Computador Mac

## Verificar a Porta

```Shell
ls /dev/tty.*
```

O resultado é semelhante à imagem abaixo; qualquer uma das duas portas funciona

![A imagem mostra a saída no terminal do Mac após executar o comando "ls /dev/tty.*", listando várias portas. Duas delas, "/dev/tty.usbmodem5AAF2194741" e "/dev/tty.wchusbserial5AAF2194741", estão destacadas com caixas vermelhas. O contexto menciona a verificação de portas, e o resultado é semelhante a esta imagem; qualquer uma das duas portas pode ser usada. A imagem apresenta visualmente as informações de porta a serem verificadas, estando intimamente relacionada ao conteúdo "Verificar a Porta" acima, e é uma exibição do resultado da operação de verificação de portas.](../../en/images/d09-01.png)

## Conceder Permissões à Porta

Conceda a todos os usuários permissão de leitura e escrita nesses dispositivos seriais

```Shell
chmod 666 /dev/tty.*
```

## Registrar Minhas Portas

Braço Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Braço Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Por Que Existem Duas Portas no Mac?

A placa controladora de servos que usamos é **reconhecida pelo macOS como dois tipos diferentes de driver serial ao mesmo tempo**, por isso duas portas são exibidas:

- Uma é o driver serial genérico padrão do sistema (`/dev/tty.usbmodemxxxx`)
- A outra é o driver serial dedicado fornecido pelo fabricante do chip (por exemplo, "wch" aqui se refere ao chip CH340/CH341 da Nanjing Qinheng) (`/dev/tty.wchusbserialxxxx`)

Isso é normal — **as duas portas correspondem, na verdade, ao mesmo dispositivo de hardware**, e você pode conectar e se comunicar por qualquer uma delas (por exemplo, basta escolher qualquer uma das portas no software que controla o braço robótico).

Se você encontrar um erro ao usar uma das portas mais tarde, tente trocar para a outra.
