[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | Français | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | [한국어](../ko/resources.md) | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>Quelques ressources</title>

# [Fichier URDF](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf) officiel de Lerbot



## URDF Studio

https://urdf.d-robotics.cc/



## Contrôle en simulation ROS2 (à implémenter soi-même)

https://github.com/holmsslk/so-arm-moveit-hardware



## Interface graphique officielle de LeRobot

https://github.com/huggingface/leLab

LeLab est une application web qui réunit tout le flux de travail de LeRobot — étalonnage, téléopération, enregistrement, entraînement, relecture — dans une seule interface de navigateur. Il suffit de connecter le bras robotique, d'ouvrir l'application et de commencer à travailler. Aucun travail laborieux en ligne de commande et aucune saisie au clavier ne sont nécessaires.

🤗 Le point d'entrée web natif de LeRobot, conçu pour permettre aux nouveaux utilisateurs de passer de la « sortie du carton » à « l'entraînement de leur première politique » en quelques minutes.

🤗 Installation et exécution de l'ensemble avec une seule commande.



# Piloter le bras Follower depuis un téléphone

https://huggingface.co/docs/lerobot/main/en/phone_teleop





## Développement robotique dans le cloud : appareils ROS 2 et simulation LeRobot Isaac Sim et streaming de données sur AWS

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## Configuration des ID de servomoteurs et étalonnage du centre dans l'interface web

https://bambot.org/feetech.js?lang=zh

1. Saisissez 0 ou 1 selon le modèle du servomoteur, puis cliquez sur « Connect »

![L'image montre l'interface de connexion pour configurer les ID de servomoteurs et l'étalonnage du centre dans l'interface web. L'interface comporte une section « Connect », avec une liste déroulante de débit en bauds (1 000 000 bps, Index 0), une liste déroulante de protocole (0=STS/SMS, 1=SCS, actuellement 0), un bouton Connect (vert, marqué du chiffre 2) et un affichage d'état (rouge, indiquant « Disconnected »). Cette image fait écho au texte précédent « Saisissez 0 ou 1 selon le modèle du servomoteur, puis cliquez sur Connect » et présente visuellement les réglages de l'interface pour l'opération de connexion.](../en/images/d58-01.png)

2. Scannez les servomoteurs d'ID 1\~6 ; utilisez FOUND dans les résultats du scan pour confirmer le servomoteur d'ID correspondant. Par exemple, dans l'image, le servomoteur d'ID 1 a été trouvé

![Il s'agit d'une capture d'écran de l'interface de scan des servomoteurs du logiciel lié à Lerbot ; son contenu central est le processus et le résultat du scan des servomoteurs. L'interface comporte des champs « Start ID » et « End ID », remplis respectivement avec 1 et 6, ainsi qu'un bouton « Start scan », avec la zone d'affichage des résultats du scan en dessous, correspondant au contenu sur les ID de servomoteurs scannés mentionné dans le document. Le journal des résultats du scan montre que les informations du servomoteur n'ont été obtenues avec succès que lors du scan de l'ID 1, affichant « FOUND: Pos-1230, Mode=0, BaudIdx=0 », tandis que le scan des ID 2 à 6 a produit à chaque fois une erreur de lecture de position anormale et d'absence de paquet de statut.](../en/images/d58-02.png)

3. Configuration des ID et étalonnage du centre

① Réglez la saisie de l'ID du servomoteur courant sur l'ID du servomoteur scanné

② Saisissez un nombre dans « ID management » et cliquez sur « Change ID » pour définir l'ID

③ Étalonnage du centre (le centre du servomoteur STS3215 est 2047, celui du servomoteur SCS0009 est 511)

Servomoteur STS : saisissez 2047 dans « Position control » et cliquez sur « Set »

Servomoteur SCS : saisissez 511 dans « Position control » et cliquez sur « Set »

![L'image montre l'interface de contrôle d'un servomoteur unique dans URDF Studio, l'outil officiel de Lerbot. Le « Current servo ID » est affiché comme 1 ; en dessous, dans « ID management », on peut saisir un nombre et cliquer sur « Change ID » ; dans la zone « Position Control » se trouve un bouton « Read position » affichant la position 2047, à côté d'un bouton « Set ». Cette image est étroitement liée au contexte et présente visuellement les étapes mentionnées : régler la saisie de l'ID du servomoteur courant sur l'ID du servomoteur scanné, saisir un nombre dans « ID management » et cliquer sur « Change ID » pour définir l'ID, et saisir 2047 dans « Position Control » et cliquer sur « Set » pour un servomoteur STS.](../en/images/d58-03.png)
