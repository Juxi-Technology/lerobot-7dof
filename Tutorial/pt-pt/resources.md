[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | Português (PT)

<title>Alguns Recursos</title>

# [Ficheiro URDF](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf) oficial do Lerbot



## URDF Studio

https://urdf.d-robotics.cc/



## Controlo de simulação ROS2 (implemente-o você mesmo)

https://github.com/holmsslk/so-arm-moveit-hardware



## Interface gráfica oficial do LeRobot

https://github.com/huggingface/leLab

O LeLab é uma aplicação web que reúne todo o fluxo de trabalho do LeRobot — calibração, teleoperação, gravação, treino, reprodução — numa única interface de browser. Basta ligar o braço robótico, abrir a aplicação e pode começar a trabalhar. Sem trabalho fastidioso de linha de comandos e sem necessidade de introduzir nada pelo teclado.

🤗 O ponto de entrada web nativo do LeRobot, concebido para permitir que os novos utilizadores passem de "pronto a usar" a "treinar a sua primeira política" em questão de minutos.

🤗 Instale e execute tudo com um único comando.



# Controlar o Braço Follower a Partir de um Telefone

https://huggingface.co/docs/lerobot/main/en/phone_teleop





## Desenvolvimento de robótica na nuvem: dispositivos ROS 2 e simulação e streaming de dados LeRobot no Isaac Sim na AWS

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## Definir IDs dos servos e calibração de centro na interface web

https://bambot.org/feetech.js?lang=zh

1. Introduza 0 ou 1 consoante o modelo do servo e, em seguida, clique em "Connect"

![A imagem mostra a interface de ligação para definir IDs dos servos e a calibração de centro na interface web. A interface tem uma secção "Connect" com uma lista pendente de taxa de baud (1,000,000 bps, Index 0), uma lista pendente de extremidade de protocolo (0=STS/SMS, 1=SCS, atualmente definida para 0), um botão Connect (verde, assinalado com o número 2) e um visor de estado (vermelho, a mostrar "Disconnected"). Esta imagem ecoa o texto anterior "Introduza 0 ou 1 consoante o modelo do servo e, em seguida, clique em 'Connect'", apresentando visualmente as definições da interface para a operação de ligação.](../en/images/d58-01.png)

2. Digitalize os servos com IDs 1\~6; utilize FOUND nos resultados da digitalização para confirmar o servo com o ID correspondente. Por exemplo, na imagem foi encontrado o servo com ID 1

![Esta é uma captura de ecrã da interface de digitalização de servos do software relacionado com o Lerbot; o conteúdo central é o processo e o resultado da digitalização dos servos. A interface tem campos de entrada "Start ID" e "End ID", preenchidos com 1 e 6, respetivamente, além de um botão "Start scan", com a área de resultados da digitalização correspondente por baixo — correspondendo ao conteúdo sobre IDs de servos digitalizados mencionado no documento. O registo dos resultados mostra que só ao digitalizar o ID1 se obteve com êxito a informação do servo, apresentando "FOUND: Pos-1230, Mode=0, BaudIdx=0", enquanto ao digitalizar do ID2 ao ID6 surgiram erros de leitura de posição anómala e de ausência de pacote de estado.](../en/images/d58-02.png)

3. Definição do ID e calibração de centro

① Defina a introdução do ID do servo atual para o ID do servo digitalizado

② Introduza um número em "ID management" e clique em "Change ID" para definir o ID

③ Calibração de centro (o centro do servo STS3215 é 2047, o centro do servo SCS0009 é 511)

Servo STS: introduza 2047 em "Position control" e clique em "Set"

Servo SCS: introduza 511 em "Position control" e clique em "Set"

![A imagem mostra a interface de controlo de um único servo no URDF Studio oficial do Lerbot. O "Current servo ID" aparece como 1; abaixo, em "ID management", é possível introduzir um número e clicar em "Change ID"; na área "Position Control" há um botão "Read position" que mostra a posição 2047, ao lado de um botão "Set". Esta imagem está estreitamente ligada ao contexto, apresentando visualmente os passos aí mencionados: definir a introdução do ID do servo atual para o ID do servo digitalizado, introduzir um número em "ID management" e clicar em "Change ID" para definir o ID, e introduzir 2047 em "Position control" e clicar em "Set" para um servo STS.](../en/images/d58-03.png)
