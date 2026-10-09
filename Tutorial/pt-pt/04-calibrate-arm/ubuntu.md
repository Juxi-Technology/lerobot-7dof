[English](../en/04-calibrate-arm/ubuntu.md) | [简体中文](../zh-hans/04-calibrate-arm/ubuntu.md) | [繁體中文](../zh-hant/04-calibrate-arm/ubuntu.md) | [Deutsch](../de/04-calibrate-arm/ubuntu.md) | [Español](../es/04-calibrate-arm/ubuntu.md) | [Français](../fr/04-calibrate-arm/ubuntu.md) | [Italiano](../it/04-calibrate-arm/ubuntu.md) | [日本語](../ja/04-calibrate-arm/ubuntu.md) | [한국어](../ko/04-calibrate-arm/ubuntu.md) | [Português (BR)](../pt-br/04-calibrate-arm/ubuntu.md) | Português (PT)

# Computador Ubuntu

## Conceder permissões à porta

Conceder a todos os utilizadores permissão para ler e escrever nestes dispositivos série

```Shell
sudo chmod 666 /dev/ttyACM*
```

## Calibrar o braço Follower

```Shell
lerobot-calibrate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM0 \
    --robot.id=my_follower_arm
```

![A imagem mostra a interface do terminal num computador Ubuntu a executar o comando "lerobot-calibrate" para calibrar o braço Follower. Apresenta a informação de ligação do Follower, os nomes das articulações e os valores dos limites superior/inferior. A informação principal inclui: premir Enter para iniciar a calibração, percorrer cada articulação pelos seus limites superior e inferior, premir Enter para terminar a calibração; e "Calibration saved to" e outra informação sobre o caminho do ficheiro de calibração. Esta imagem está estreitamente relacionada com os passos de calibração do braço Follower, apresentando visualmente o feedback do terminal durante a calibração.](../../en/images/d12-01.png)

## Calibrar o braço Leader

```Shell
lerobot-calibrate \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM1 \
    --teleop.id=my_leader_arm
```

![A imagem mostra a interface num computador Ubuntu depois de conceder permissões à porta. A linha de comandos introduziu "sudo chmod 666 /dev/ttyACM*" e, após a execução, apresentou informação como "zihao_leader_arm". Em baixo estão instruções como "press Enter to start calibration", "turn each joint through its upper and lower limits in turn" e "press Enter to finish calibration", juntamente com "Calibration saved to" e outra informação de caminho relacionada com a calibração. Esta imagem corresponde à secção "Calibrar o braço Leader", apresentando visualmente a interface de preparação antes da calibração.](../../en/images/d12-02.png)

## Ver o ficheiro de configuração de calibração

```Shell
sudo nano /home/tommy/.cache/huggingface/lerobot/calibration/robots/so101_follower/my_follower_arm.json
```

![A imagem mostra o conteúdo do ficheiro "zihao_follower_arm.json" apresentado no terminal do Ubuntu. O ficheiro contém informação de configuração de vários braços, como shoulder_pan, shoulder_lift, elbow_flex e wrist_flex, tendo cada braço parâmetros como id, drive_mode, homing_offset, range_min e range_max. Esta imagem está relacionada com a secção "Ver o ficheiro de configuração de calibração", apresentando visualmente a informação de parâmetros específica do ficheiro de calibração e ajudando os utilizadores a compreender a configuração de cada braço.](../../en/images/d12-03.png)



## Notas

### ① Um braço deixa de se mover após atingir um limite

É necessário recalibrá-lo

<figure view-type="Preview">[Attachment: wx_camera_1768098182808.mp4](../../en/images/wx_camera_1768098182808.mp4)</figure>

### ② Servos não encontrados

![Esta é uma captura de ecrã que mostra uma interface de erro no terminal do Ubuntu, correspondente à nota "servos não encontrados". A interface reporta um RuntimeError, concretamente "FeetechMotorsBus motor check failed on port '/dev/tty.usbmodem5AAF2193061:'", ou seja, a verificação dos servos falhou. Lista também a informação dos servos esperados, com IDs de motor esperados de 1-6 e modelo esperado 777, mas a lista de motores efetivamente encontrados está vazia; combinado com o contexto, este erro é causado por os servos não estarem alimentados.](../../en/images/d12-04.png)

A alimentação dos servos não está ligada
