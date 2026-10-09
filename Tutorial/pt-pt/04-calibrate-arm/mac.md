[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | [Português (BR)](../pt-br/04-calibrate-arm/mac.md) | Português (PT)

# Computador Mac

## Rever os números das portas

Braço Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Braço Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Calibrar o braço Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![A imagem mostra a interface da linha de comandos para calibrar os servos SO101 num Mac. O comando é "lerobot-calibrate", com parâmetros que incluem robot.type, robot.port e robot.id. A interface apresenta informação de configuração do robô, como "zihao_follower_arm". Em baixo, pede para premir "c" e Enter para iniciar a calibração, e mostra também mensagens como "zihao_follower_arm SO101Follower connected". Esta imagem corresponde à secção "Calibrar o braço Follower", apresentando visualmente o comando de calibração e o feedback da interface.](../../en/images/d13-01.png)

![A imagem mostra a interface da linha de comandos de uma operação de calibração do LeRobot no Ubuntu. O comando é "lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm", apresentando a informação de calibração do Follower, incluindo a posição mínima, máxima e atual de cada articulação. Instruções de operação fundamentais estão realçadas com retângulos vermelhos, como "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" e "press Enter to finish calibration", correspondendo aos passos de calibração descritos no contexto.](../../en/images/d13-02.png)

## Calibrar o braço Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![A imagem mostra a interface da linha de comandos de uma calibração do LeRobot no Ubuntu. A linha de comandos executou operações como "sudo chmod 666 /dev/ttyACM*" e "lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1", apresentando a informação dos números de porta dos braços Follower e Leader. A interface pede ainda para premir Enter para iniciar a calibração, percorrer cada articulação pelos seus limites superior e inferior, e premir Enter para terminar, e por fim mostra o caminho onde o ficheiro de configuração de calibração é guardado. Esta imagem está relacionada com o conteúdo de calibração do LeRobot, apresentando visualmente os passos de calibração.](../../en/images/d13-03.png)

## Ver o ficheiro de configuração de calibração

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## Erros comuns

- Não é possível encontrar um ou vários dos servos

![A imagem mostra a informação dos parâmetros dos servos apresentada durante a calibração do SO Follower. No topo mostra informação de ligação e uma indicação de calibração a pedir para mover o Follower para o meio do seu intervalo de movimento e premir ENTER, percorrer depois todos os intervalos de movimento das articulações por ordem, registar as posições e premir ENTER para parar. A tabela abaixo lista os valores NAME, MIN, POS e MAX de servos como shoulder_pan, shoulder_lift, elbow_flex, wrist_flex e gripper. Esta imagem está relacionada com a calibração do braço Follower, apresentando visualmente os parâmetros durante a calibração.](../../en/images/d13-04.png)



## Notas

### ① Um braço deixa de se mover após atingir um limite

É necessário recalibrá-lo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos não encontrados

![A imagem mostra a interface do terminal do Mac com uma mensagem de erro da execução do código de robô do LeRobot. O erro indica que a verificação dos motores FeetechMotorsBus falhou na porta '/dev/tty.usbmodem5AAF2193061', com os IDs de motor de -1 a -6 ausentes e um modelo esperado de 777. Lista também a lista completa de motores esperados e a lista completa de motores encontrados. Esta imagem está relacionada com a secção "Erros comuns", apresentando visualmente como o problema "servos não encontrados" aparece como um erro de execução.](../../en/images/d13-05.png)

A alimentação dos servos não está ligada
