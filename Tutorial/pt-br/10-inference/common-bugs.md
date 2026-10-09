[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | Português (BR) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# Bugs Comuns e Soluções

## A Captura da Câmera Falha

![A imagem mostra a saída do terminal quando o código relacionado ao robô LeRobot é executado. Nela, os logs `INFO` mostram a abertura da câmera OpenCV e a desconexão do Follower; o log `ERROR` aponta que, no arquivo `camera_opencv.py`, a função `read` lançou um `RuntimeError` por causa de `OpenCVCamera(0) read failed`. Esta imagem está relacionada ao problema "A captura da câmera falha" do documento, mostrando visualmente a situação de erro que ocorre durante a execução do código e ajudando a entender como esse problema se manifesta na prática.](../../en/images/d55-01.png)

Verifique se a fiação da câmera do pulso está solta, especialmente a extremidade próxima à câmera — esse conector é muito propenso a mau contato

## A Câmera se Desconecta

![A imagem mostra a interface de código e as mensagens de erro do problema de falha na captura da câmera ocorrido durante a execução do LeRobot HF. Na parte superior há o aviso de falha na conexão da câmera OpenCV; na parte inferior está a pilha de erros da função `record_loop` do arquivo `lerobot_record.py`, apontando que ocorreu um `TimeoutError` em `async_read`, indicando que a câmera não obteve um quadro em 200ms. Esta imagem corresponde ao contexto do problema "A captura da câmera falha" do documento, mostrando visualmente a manifestação específica desse problema no nível do código.](../../en/images/d55-02.png)

Reinicie a linha de comando

## Problema de Comunicação com o Servo 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![Esta imagem é um trecho de log de erro do terminal, correspondendo ao conteúdo relacionado ao Problema de Comunicação com o Servo 1 mencionado no documento. O log exibe o caminho específico do arquivo de código; nas linhas 1069 e 1181 do arquivo `lerobot/src/lerobot/motors/motors_bus.py`, registra a mensagem de erro `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`, além de marcar o motivo do erro `There is no status packet!`, apresentando por completo os detalhes do erro do Problema de Comunicação com o Servo 1 mencionado no documento.](../../en/images/fix-04.png)

Solução: altere todos os `num_retry` no código de `lerobot/src/lerobot/motors/motors_bus.py` para 99, especialmente o correspondente à linha do erro

![A imagem mostra o conteúdo do arquivo de código `motors_bus.py` do projeto LeRobot. O método `write` da classe `MotorsBusABC` está destacado, com a variável `num_retry` alterada para `99`. Esta imagem está relacionada à seção "Problema de Comunicação com o Servo 1" do documento, correspondendo à operação mencionada na solução de alterar todos os `num_retry` no código de `lerobot/src/lerobot/motors/motors_bus.py` para 99, especialmente a parte do código correspondente à linha do erro.](../../en/images/d55-03.png)

## Problema de Comunicação com o Servo 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![Esta imagem mostra um log de erro, pertencente ao conteúdo de erro correspondente ao "Problema de Comunicação com o Servo 2" do documento; especificamente, durante a execução do código ocorreu um erro chamado ConnectionError, manifestado como Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries, com a mensagem de erro associada indicando ainda There is no status packet, além de marcar o caminho do código onde o erro foi acionado. Este log de erro corresponde completamente à descrição do "Problema de Comunicação com o Servo 2" do documento e é o registro real do erro desse problema.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra a interface de execução de uma operação de linha de comando usando o terminal zsh no sistema macOS. O terminal exibe vários caminhos de arquivos e conteúdos de arquivos de código, entre os quais o arquivo `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` contém a mensagem de erro `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`. Esta imagem está relacionada ao conteúdo do "Problema de Comunicação com o Servo 2" do documento, mostrando visualmente a situação da interface do terminal no momento do erro.](../../en/images/fix-05.png)
</column>
</grid>

Solução: recalibre o braço robótico
