[English](../en/04-calibrate-arm/mac.md) | [简体中文](../zh-hans/04-calibrate-arm/mac.md) | [繁體中文](../zh-hant/04-calibrate-arm/mac.md) | [Deutsch](../de/04-calibrate-arm/mac.md) | [Español](../es/04-calibrate-arm/mac.md) | [Français](../fr/04-calibrate-arm/mac.md) | [Italiano](../it/04-calibrate-arm/mac.md) | [日本語](../ja/04-calibrate-arm/mac.md) | [한국어](../ko/04-calibrate-arm/mac.md) | Português (BR) | [Português (PT)](../pt-pt/04-calibrate-arm/mac.md)

# Computador Mac

## Revisar os Números das Portas

Braço Follower:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Braço Leader:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Calibrar o Braço Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem5AAF2193061 \
    --robot.id=zihao_follower_arm
```

![A imagem mostra a interface de linha de comando para calibrar os servos do SO101 no Mac. O comando é "lerobot-calibrate", com parâmetros incluindo robot.type, robot.port e robot.id. A interface exibe informações de configuração do robô, como "zihao_follower_arm". Abaixo, ela solicita que você pressione "c" e Enter para iniciar a calibração, e também mostra mensagens como "zihao_follower_arm SO101Follower connected". Esta imagem corresponde à seção "Calibrar o Braço Follower", apresentando visualmente o comando de calibração e o retorno da interface.](../../en/images/d13-01.png)

![A imagem mostra a interface de linha de comando para uma operação de calibração do LeRobot no Ubuntu. O comando é "lerobot calibrate --robot.port=/dev/ttyACM0 --robot.id=zihao_follower_arm", exibindo as informações de calibração do Follower, incluindo a posição mínima, máxima e atual de cada articulação. Prompts importantes de operação estão destacados com caixas vermelhas, como "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" e "press Enter to finish calibration", ecoando as etapas de calibração descritas no contexto.](../../en/images/d13-02.png)

## Calibrar o Braço Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/tty.usbmodem5AAF2194741 \
    --teleop.id=zihao_leader_arm
```

![A imagem mostra a interface de linha de comando para uma calibração do LeRobot no Ubuntu. A linha de comando executou operações como "sudo chmod 666 /dev/ttyACM*" e "lerobot_calibrate --teleop.type=so101_leader --teleop.port=/dev/ttyACM1", exibindo as informações do número de porta dos braços Follower e Leader. A interface também solicita que você pressione Enter para iniciar a calibração, passe cada articulação pelos seus limites superior e inferior em sequência, e pressione Enter para finalizar, e por fim mostra o caminho onde o arquivo de configuração da calibração é salvo. Esta imagem está relacionada ao conteúdo da calibração do LeRobot, apresentando visualmente as etapas de calibração.](../../en/images/d13-03.png)

## Visualizar o Arquivo de Configuração da Calibração

```Shell
sudo nano /Users/tommy/.cache/huggingface/lerobot/calibration/teleoperators/so101_leader/zihao_leader_arm.json
```



## Problemas Comuns

- Um ou vários dos servos não podem ser encontrados

![A imagem mostra as informações dos parâmetros dos servos exibidas durante a calibração do SO Follower. No topo, há informações de conexão e um prompt de calibração pedindo que você mova o Follower para o meio da sua faixa de movimento e pressione ENTER, depois passe por toda a faixa de movimento de todas as articulações em ordem, registre as posições e pressione ENTER para parar. A tabela abaixo lista os valores NAME, MIN, POS e MAX para servos como shoulder_pan, shoulder_lift, elbow_flex, wrist_flex e gripper. Esta imagem está relacionada à calibração do braço Follower, apresentando visualmente os parâmetros durante a calibração.](../../en/images/d13-04.png)



## Observações

### ① Um braço para de se mover depois de atingir um limite

Ele precisa ser recalibrado

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos não encontrados

![A imagem mostra a interface do terminal do Mac com uma mensagem de erro da execução do código do robô LeRobot. O erro informa que a verificação dos motores do FeetechMotorsBus falhou na porta '/dev/tty.usbmodem5AAF2193061', com os IDs de motor de -1 a -6 ausentes e um modelo esperado de 777. Também lista a lista completa de motores esperados e a lista completa de motores encontrados. Esta imagem está relacionada à seção "Problemas Comuns", apresentando visualmente como o problema de "servos não encontrados" aparece como um erro em tempo de execução.](../../en/images/d13-05.png)

A alimentação dos servos não está conectada
