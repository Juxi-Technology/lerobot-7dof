[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | Português (BR) | [Português (PT)](../pt-pt/04-calibrate-arm/ubuntu.md)

# Computador Ubuntu

## Conceder Permissões à Porta

Conceda a todos os usuários permissão de leitura e escrita nesses dispositivos seriais

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Calibrar o Braço Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![A imagem mostra a interface do terminal em um computador Ubuntu executando o comando "lerobot-calibrate" para calibrar o braço Follower. Ela exibe as informações de conexão do Follower, os nomes das articulações e os valores dos limites superior e inferior. As informações-chave incluem: pressione Enter para iniciar a calibração, passe cada articulação pelos seus limites superior e inferior em sequência, pressione Enter para finalizar a calibração; e "Calibration saved to" e outras informações do caminho do arquivo de calibração. Esta imagem está intimamente relacionada às etapas de calibração do braço Follower, apresentando visualmente o retorno do terminal durante a calibração.](../../en/images/d12-01.png)

## Calibrar o Braço Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![A imagem mostra a interface em um computador Ubuntu após conceder permissões à porta. A linha de comando digitou "sudo chmod 666 /dev/ttyACM*" e, após a execução, exibiu informações como "zihao_leader_arm". Abaixo há prompts como "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" e "press Enter to finish calibration", juntamente com "Calibration saved to" e outras informações de caminho relacionadas à calibração. Esta imagem corresponde à seção "Calibrar o Braço Leader", apresentando visualmente a interface de preparação antes da calibração.](../../en/images/d12-02.png)

## Visualizar o Arquivo de Configuração da Calibração

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![A imagem mostra o conteúdo do arquivo "zihao_follower_arm.json" exibido no terminal do Ubuntu. O arquivo contém informações de configuração de vários braços, como shoulder_pan, shoulder_lift, elbow_flex e wrist_flex, cada braço tendo parâmetros como id, drive_mode, homing_offset, range_min e range_max. Esta imagem está relacionada à seção "Visualizar o Arquivo de Configuração da Calibração", apresentando visualmente as informações específicas dos parâmetros no arquivo de calibração e ajudando os usuários a entender a configuração de cada braço.](../../en/images/d12-03.png)



## Observações

### ① Um braço para de se mover depois de atingir um limite

Ele precisa ser recalibrado

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos não encontrados

![Esta é uma captura de tela mostrando uma interface de erro no terminal do Ubuntu, correspondendo à observação "servos não encontrados". A interface relata um RuntimeError, especificamente "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'", ou seja, a verificação dos servos falhou. Ela também lista as informações dos servos esperados, com IDs de motor esperados de 1 a 6 e modelo esperado 777, mas a lista de motores realmente encontrados está vazia; combinado com o contexto, este erro é causado pelos servos não estarem energizados.](../../en/images/d12-04.png)

A alimentação dos servos não está conectada
