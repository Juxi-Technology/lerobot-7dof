[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | Português (PT)

# Computador Mac

## Verificar a porta

```Shell
ls /dev/tty.*
```

O resultado é semelhante ao da imagem abaixo; qualquer uma das duas portas funciona

![A imagem mostra a saída no terminal do Mac após executar o comando "ls /dev/tty.*", listando várias portas. Duas delas, "/dev/tty.usbmodem5AAF2194741" e "/dev/tty.wchusbserial5AAF2194741", estão realçadas com retângulos vermelhos. O contexto menciona a verificação de portas, e o resultado é semelhante a esta imagem; pode utilizar-se qualquer uma das duas portas. A imagem apresenta visualmente a informação das portas a verificar, estreitamente relacionada com o conteúdo "Verificar a porta" acima, e é uma demonstração do resultado da operação de verificação de portas.](../../en/images/d09-01.png)

## Conceder permissões à porta

Conceder a todos os utilizadores permissão para ler e escrever nestes dispositivos série

```Shell
chmod 666 /dev/tty.*
```

## Registar as minhas portas

Braço Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Braço Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Porque é que existem duas portas num Mac?

A placa controladora dos servos que utilizamos é **reconhecida pelo macOS como dois tipos diferentes de controladores série ao mesmo tempo**, pelo que são apresentadas duas portas:

- Uma é o controlador série genérico predefinido do sistema (`/dev/tty.usbmodemxxxx`)
- A outra é o controlador série dedicado fornecido pelo fabricante do chip (por exemplo, aqui "wch" refere-se ao chip CH340/CH341 da Nanjing Qinheng) (`/dev/tty.wchusbserialxxxx`)

Isto é normal — **as duas portas correspondem, na realidade, ao mesmo dispositivo de hardware**, e pode ligar-se e comunicar através de qualquer uma delas (por exemplo, basta escolher qualquer uma das portas no software que controla o braço do robô).

Se mais tarde encontrar um erro ao utilizar uma porta, tente mudar para a outra.
