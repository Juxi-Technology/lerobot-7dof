[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | Français | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | [한국어](../ko/10-inference/common-bugs.md) | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# Bogues courants et correctifs

## La capture de la caméra échoue

![L'image montre la sortie du terminal lors de l'exécution du code du robot LeRobot. Les journaux `INFO` montrent l'ouverture de la caméra OpenCV et la déconnexion du Follower ; le journal `ERROR` signale que dans le fichier `camera_opencv.py`, la fonction `read` a levé une `RuntimeError` à cause de `OpenCVCamera(0) read failed`. Cette image concerne le problème « La capture de la caméra échoue » du document, présentant visuellement l'erreur qui apparaît à l'exécution du code et aidant à comprendre comment le problème se manifeste en pratique.](../../en/images/d55-01.png)

Vérifiez si le câble de la caméra du poignet est desserré, en particulier l'extrémité proche de la caméra — ce connecteur est très sujet aux mauvais contacts

## La caméra se déconnecte

![L'image montre le code et les informations d'erreur du problème d'échec de capture de la caméra survenant pendant l'exécution de LeRobot HF. En haut s'affiche un échec de connexion de la caméra OpenCV ; en dessous se trouve la pile d'erreurs de la fonction `record_loop` du fichier `lerobot_record.py`, indiquant qu'une `TimeoutError` s'est produite lors de `async_read` et que la caméra n'a pas obtenu d'image en 200 ms. Cette image correspond au problème « La capture de la caméra échoue » du document et présente visuellement la manifestation concrète de ce problème au niveau du code.](../../en/images/d55-02.png)

Redémarrez la ligne de commande

## Problème de communication avec le servo 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![Cette image est un journal d'erreur du terminal, correspondant au contenu « Problème de communication avec le servo 1 » mentionné dans le document. Le journal montre des chemins de fichiers de code précis : les lignes 1069 et 1181 du fichier `lerobot/src/lerobot/motors/motors_bus.py` enregistrent l'erreur `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries`, et précise aussi la cause de l'erreur `There is no status packet!`, présentant complètement les détails de l'erreur du problème de communication avec le servo 1 mentionné dans le document.](../../en/images/fix-04.png)

Le correctif : passez chaque `num_retry` du code situé à `lerobot/src/lerobot/motors/motors_bus.py` à 99, en particulier celui de la ligne qui provoque l'erreur

![L'image montre le contenu du fichier de code `motors_bus.py` du projet LeRobot. La méthode `write` de la classe `MotorsBusABC` est mise en évidence, avec la variable `num_retry` modifiée à `99`. Cette image concerne la section « Problème de communication avec le servo 1 » du document, correspondant au correctif consistant à passer chaque `num_retry` du code situé à `lerobot/src/lerobot/motors/motors_bus.py` à 99, en particulier celui de la ligne qui provoque l'erreur.](../../en/images/d55-03.png)

## Problème de communication avec le servo 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![Cette image montre un journal d'erreur appartenant au contenu d'erreur « Problème de communication avec le servo 2 » du document. Plus précisément, une ConnectionError s'est produite lors de l'exécution du code : Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries ; le message d'erreur associé indique aussi There is no status packet, et le chemin du code à l'origine de l'erreur est également indiqué. Ce journal d'erreur correspond exactement à la description du « Problème de communication avec le servo 2 » dans le document et constitue l'enregistrement réel de cette erreur.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![L'image montre une session en ligne de commande dans le terminal zsh sous macOS. Le terminal affiche plusieurs chemins de fichiers et contenus de fichiers de code ; le fichier `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` contient l'erreur `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!`. Cette image concerne le contenu « Problème de communication avec le servo 2 » du document et présente visuellement l'interface du terminal au moment de l'erreur.](../../en/images/fix-05.png)
</column>
</grid>

Solution : recalibrez le bras robotisé
