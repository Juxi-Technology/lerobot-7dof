[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | Português (BR) | [Português (PT)](../pt-pt/resources.md)

<title>Alguns recursos</title>

# Arquivo [URDF](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf) oficial do Lerbot



## URDF Studio

https://urdf.d-robotics.cc/



## Controle de Simulação ROS2 (implemente você mesmo)

https://github.com/holmsslk/so-arm-moveit-hardware



## Interface Gráfica Oficial do LeRobot

https://github.com/huggingface/leLab

O LeLab é um aplicativo web que reúne todo o fluxo de trabalho do LeRobot — calibração, teleoperação, gravação, treinamento, reprodução — em uma única interface de navegador. Basta conectar o braço robótico, abrir o aplicativo e começar a trabalhar. Sem trabalho complicado na linha de comando e sem precisar de teclado.

🤗 O ponto de entrada web nativo do LeRobot, projetado para levar novos usuários de "pronto para usar" a "treinar sua primeira policy" em poucos minutos.

🤗 Instale e execute tudo com um único comando.



# Controlar o Braço Follower pelo Celular

https://huggingface.co/docs/lerobot/main/en/phone_teleop




## Desenvolvimento de Robótica na Nuvem: Simulação LeRobot de Dispositivos ROS 2 com o Isaac Sim e Fluxo de Dados na AWS

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## Configurar IDs de Servo e Calibração de Centro na Interface Web

https://bambot.org/feetech.js?lang=zh

1. Digite 0 ou 1 conforme o modelo do servo e clique em "Connect"

![A imagem mostra a interface de conexão ao definir IDs de servo e fazer a calibração de centro na interface web. Na interface há uma seção "Connect", incluindo um seletor de taxa de baud (1,000,000 bps, Index 0), um seletor de protocolo (0=STS/SMS, 1=SCS, com 0 selecionado no momento), um botão de conexão (verde, marcado com o número 2) e um indicador de status (vermelho, exibindo "Desconectado"). Esta imagem corresponde ao texto acima, "digite 0 ou 1 conforme o modelo do servo e clique em Connect", apresentando visualmente as configurações da interface da operação de conexão.](../en/images/d58-01.png)

2. Escaneie os servos com ID 1\~6; você pode confirmar o servo com o ID correspondente pelo FOUND nos resultados da varredura. Por exemplo, na imagem o servo de ID 1 foi escaneado

![Esta é uma captura de tela da interface de varredura de servos do software relacionado ao Lerbot, cujo conteúdo central é o processo e o resultado da operação de varredura de servos. A interface possui campos de entrada "Starting ID" e "Ending ID", preenchidos com 1 e 6, respectivamente, além de um botão "Start scan", abaixo do qual está a área de exibição do resultado da varredura correspondente — posição que corresponde ao conteúdo mencionado no documento sobre o ID do servo ser escaneado. Os logs do resultado da varredura mostram que apenas ao escanear o ID 1 a informação do servo foi obtida com sucesso, exibindo "FOUND: Pos-1230, Mode=0, BaudIdx=0"; ao escanear do ID 2 ao ID 6, surgiram, em cada caso, mensagens de erro de leitura de posição anormal e ausência de pacote de status.](../en/images/d58-02.png)

3. Definição de ID e calibração de centro

① O ID do servo atual é definido como o ID do servo escaneado

② Digite um número em "ID management" e clique em "Change ID" para definir o ID

③ Calibração de centro (o centro do servo STS3215 é 2047 e o do servo SCS0009 é 511)

Servo STS: digite 2047 em "Position control" e clique em "Set"

Servo SCS: digite 511 em "Position control" e clique em "Set"

![A imagem mostra a interface de controle de um único servo no URDF Studio oficial do Lerbot. Na interface, o "Current servo ID" é exibido como 1; abaixo, na área "ID management", é possível digitar um número e clicar em "Change ID"; na área "Position control" há um botão "Read position", exibindo a posição 2047, ao lado de um botão "Set". Esta imagem está intimamente relacionada ao contexto, apresentando visualmente as etapas de operação mencionadas no contexto: definir o ID do servo atual como o ID escaneado, digitar um número em "ID management" e clicar em "Change ID" para definir o ID, e digitar 2047 em "Position control" e clicar em "Set" para o servo STS.](../en/images/d58-03.png)
