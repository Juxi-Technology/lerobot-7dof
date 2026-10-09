[English](../en/04-calibrate-arm/windows.md) | [简体中文](../zh-hans/04-calibrate-arm/windows.md) | [繁體中文](../zh-hant/04-calibrate-arm/windows.md) | [Deutsch](../de/04-calibrate-arm/windows.md) | [Español](../es/04-calibrate-arm/windows.md) | [Français](../fr/04-calibrate-arm/windows.md) | [Italiano](../it/04-calibrate-arm/windows.md) | [日本語](../ja/04-calibrate-arm/windows.md) | [한국어](../ko/04-calibrate-arm/windows.md) | Português (BR) | [Português (PT)](../pt-pt/04-calibrate-arm/windows.md)

# Computador Windows



<callout emoji="🚫">
Os braços Leader e Follower devem estar ambos conectados
</callout>

## Calibrar o Braço Follower

```Shell
lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm
```

![A imagem mostra a interface de linha de comando em um computador Windows executando uma calibração do lerobot. O comando é "lerobot-calibrate --robot.type=so101_follower --robot.port=COM6 --robot.id=zihao_follower_arm". A interface exibe informações de calibração, incluindo prompts como "zihao_follower_arm SO10IFollower connected", e também lista os valores NAME, MIN, POS e MAX de cada articulação do braço robótico. Durante a calibração, ela solicita que o usuário mova o braço robótico para o meio da sua faixa de movimento e pressione ENTER, enquanto registra as posições, e pressione ENTER para parar. Esta imagem está relacionada à calibração do braço Follower, mostrando as etapas específicas e o retorno da interface.](../../en/images/d14-01.png)

## Calibrar o Braço Leader

```Shell
lerobot-calibrate --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm
```

![A imagem mostra a interface de linha de comando calibrando o braço robótico com o comando lerobot-calibrate em um computador Windows. Ela exibe informações sobre a calibração dos braços Follower e Leader, incluindo o caminho de salvamento da posição de calibração, o tipo de robô, o número da porta e o ID. Também solicita que você mova o Follower para o meio da sua faixa de movimento e pressione ENTER, passe cada articulação por toda a sua faixa de movimento, registre as posições e pressione ENTER para parar. Na parte inferior, exibe o nome, o valor mínimo, a posição atual e o valor máximo de cada articulação.](../../en/images/d14-02.png)

## Onde os Arquivos São Exportados

C:\Users\40743\.cache\huggingface\lerobot\calibration\robots\so101_follower\my_follower_arm.json

C:\Users\40743\.cache\huggingface\lerobot\calibration\teleoperators\so101_leader\my_leader_arm.json



## Calibrando um Braço Robótico Diferente

lerobot-calibrate --robot.type=so101_follower --robot.port=COM4 --robot.id=my_follower_arm





## Observações

### ① Um braço para de se mover depois de atingir um limite

Ele precisa ser recalibrado

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos não encontrados

![A imagem mostra uma mensagem de erro ao executar o programa lerobot no macOS. Enquanto o programa é executado, aparece um RuntimeError: "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061'", indicando IDs de servo ausentes, incluindo os servos 1 a 6, todos com um número de modelo esperado de 777, mas a lista de servos realmente encontrados está vazia. Isso está relacionado à observação "servos não encontrados"; pode ser porque os servos não estão conectados, então reconecte-os e gire o conector.](../../en/images/d14-03.png)

A alimentação dos servos não está conectada; reconecte-a e gire o conector
