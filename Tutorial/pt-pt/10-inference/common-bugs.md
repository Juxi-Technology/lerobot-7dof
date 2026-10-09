[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | Português (PT)

# Erros Comuns e Correções

## A captura da câmara falha

![A imagem mostra a informação de saída do terminal durante a execução do código do robô LeRobot. Entre ela, o registo `INFO` mostra a abertura da câmara OpenCV e a desligação do Follower; o registo `ERROR` assinala que, no ficheiro `camera_opencv.py`, a função `read` lançou um `RuntimeError` devido a `OpenCVCamera(0) read failed`. Esta imagem está relacionada com o problema "A captura da câmara falha" do documento, apresentando visualmente o erro que surge durante a execução do código e ajudando a compreender como o problema se manifesta na prática.](../../en/images/d55-01.png)

Verifique se o cabo da câmara de pulso está solto, especialmente a extremidade junto à câmara — esse conector tem muita tendência a fazer mau contacto

## A câmara desliga-se

![A imagem mostra o código e a informação de erro do problema de falha de captura da câmara que ocorre durante a execução do LeRobot HF. Na parte superior aparece a indicação de falha de ligação da câmara OpenCV; abaixo está a stack de erros da função `record_loop` do ficheiro `lerobot_record.py`, indicando que ocorreu um `TimeoutError` durante o `async_read` e referindo que não foi capturado nenhum fotograma da câmara em 200 ms. Esta imagem corresponde ao contexto do problema "A captura da câmara falha" do documento, apresentando visualmente a manifestação específica do problema ao nível do código.](../../en/images/d55-02.png)

Reinicie a linha de comandos

## Problema de comunicação com o servo 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![Esta imagem é um registo de erro do terminal, correspondendo ao conteúdo sobre o problema de comunicação com o servo 1 mencionado no documento. O registo mostra caminhos de ficheiros de código específicos: nas linhas 1069 e 1181 do ficheiro `lerobot/src/lerobot/motors/motors_bus.py` é registado o erro `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`, indicando ainda a causa do erro `There is no status packet!`, apresentando na íntegra os detalhes do erro do problema de comunicação com o servo 1 referido no documento.](../../en/images/fix-04.png)

A correção: altere todos os `num_retry` no código em `lerobot/src/lerobot/motors/motors_bus.py` para 99, especialmente o da linha que dá erro

![A imagem mostra o conteúdo do ficheiro de código `motors_bus.py` no projeto LeRobot. O método `write` da classe `MotorsBusABC` está realçado, com a variável `num_retry` alterada para `99`. A imagem relaciona-se com a secção "Problema de comunicação com o servo 1", correspondendo à correção de alterar todos os `num_retry` no código em `lerobot/src/lerobot/motors/motors_bus.py` para 99, especialmente o da linha que dá erro.](../../en/images/d55-03.png)

## Problema de comunicação com o servo 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![Esta imagem mostra um registo de erro, pertencente ao conteúdo de erro correspondente ao "Problema de comunicação com o servo 2" do documento. Concretamente, durante a execução do código ocorreu um erro chamado ConnectionError, manifestado em Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries; a mensagem de erro associada indica ainda There is no status packet e assinala o caminho do código que despoletou o erro. Este registo de erro corresponde exatamente à descrição do "Problema de comunicação com o servo 2" do documento e é o registo real do erro desse problema.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![A imagem mostra uma sessão de linha de comandos no terminal zsh no sistema macOS. O terminal mostra vários caminhos de ficheiros e conteúdos de ficheiros de código, onde o ficheiro `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` contém a informação de erro `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`. Esta imagem está relacionada com o conteúdo "Problema de comunicação com o servo 2" do documento, apresentando visualmente a interface do terminal no momento do erro.](../../en/images/fix-05.png)
</column>
</grid>

Solução: recalibre o braço do robô
