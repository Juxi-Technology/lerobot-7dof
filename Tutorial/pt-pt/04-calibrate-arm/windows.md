[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | [Português (BR)](../pt-br/04-calibrate-arm/windows.md) | Português (PT)

# Computador Windows



<callout emoji="🚫">
Os braços Leader e Follower têm de estar ambos ligados
</callout>

## Calibrar o braço Follower

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![A imagem mostra a interface da linha de comandos num computador Windows a executar uma calibração do lerobot. O comando é "lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm". A interface apresenta informação de calibração, incluindo indicações como "zihao_follower_arm SO10IFollower connected", e lista também os valores NAME, MIN, POS e MAX de cada articulação do braço do robô. Durante a calibração, pede ao utilizador para mover o braço do robô para o meio do seu intervalo de movimento e premir ENTER, registando as posições, e premir ENTER para parar. Esta imagem está relacionada com a calibração do braço Follower, mostrando os passos concretos e o feedback da interface.](../../en/images/d14-01.png)

## Calibrar o braço Leader

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![A imagem mostra a interface da linha de comandos a calibrar o braço do robô com o comando lerobot-calibrate num computador Windows. Apresenta informação sobre a calibração dos braços Follower e Leader, incluindo o caminho de gravação da posição de calibração, o tipo de robô, o número de porta e o ID. Pede ainda para mover o Follower para o meio do seu intervalo de movimento e premir ENTER, percorrer cada articulação por toda a sua amplitude de movimento, registar as posições e premir ENTER para parar. Na parte inferior apresenta o nome, o valor mínimo, a posição atual e o valor máximo de cada articulação.](../../en/images/d14-02.png)

## Onde são exportados os ficheiros

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## Calibrar um braço de robô diferente

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## Notas

### ① Um braço deixa de se mover após atingir um limite

É necessário recalibrá-lo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos não encontrados

![A imagem mostra uma mensagem de erro ao executar o programa lerobot no macOS. Enquanto o programa corre, aparece um RuntimeError: "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'", indicando IDs de servo em falta, incluindo os servos 1-6, todos com um número de modelo esperado de 777, mas a lista de servos efetivamente encontrados está vazia. Isto está relacionado com a nota "servos não encontrados"; pode dever-se a os servos não estarem ligados, por isso volte a ligá-los e rode o conector.](../../en/images/d14-03.png)

A alimentação dos servos não está ligada; volte a ligá-la e rode o conector
