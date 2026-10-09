[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | Français | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | [한국어](../ko/03-find-serial-port/ubuntu.md) | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# Méthode 1 : vérifier directement depuis la ligne de commande Linux

## Vérifier les ports des périphériques série

```Shell
ls /dev/ttyACM*
```

## Connecter les ports USB de l'ordinateur et du bras robotisé

Branchez d'abord le bras Follower, puis le bras Leader

![L'image montre le processus de vérification des ports des périphériques série avec la ligne de commande Linux sous Ubuntu. Elle affiche d'abord « rien de branché », puis après le branchement du bras Follower « /dev/ttyACM0 » apparaît ; ensuite, après le branchement du bras Leader, « /dev/ttyACM1 » apparaît. L'image est étroitement liée au contexte et présente visuellement comment les ports des périphériques série sont vus depuis la ligne de commande après la connexion des ports USB de l'ordinateur et du bras robotisé, ce qui aide à expliquer comment vérifier les ports des périphériques série depuis la ligne de commande Linux.](../../en/images/d08-01.png)

# Méthode 2 : l'outil officiel LeRobot

```Shell
lerobot-find-port
```

![L'image montre l'interface en ligne de commande pour vérifier les ports des périphériques série avec l'outil officiel LeRobot sous Ubuntu. La commande est « lerobot-find-port », affichant tous les ports disponibles, dont plusieurs ports « /dev/ttyACM ». L'invite vous demande de débrancher le câble USB du Follower et de trouver son numéro de port « /dev/ttyACM0 », puis de rebrancher le câble USB. Cette image concerne la méthode deux de vérification des ports des périphériques série et présente visuellement les étapes et les résultats.](../../en/images/d08-02.png)

![L'image montre le processus d'utilisation de la commande `lerobot-find-port` pour vérifier les ports des périphériques série du bras robotisé sous Ubuntu. Après l'exécution de la commande, elle liste tous les ports disponibles, puis vous invite à débrancher le câble USB du Leader, et affiche finalement `/dev/ttyACM1` comme numéro de port du périphérique série du bras Leader. Cette image concerne la méthode deux de vérification des ports des périphériques série et présente visuellement le résultat de l'obtention des numéros de port avec l'outil officiel LeRobot.](../../en/images/d08-03.png)

# Noter mes ports

`/dev/ttyACM0` est le numéro de port du périphérique série du bras Follower

`/dev/ttyACM1` est le numéro de port du périphérique série du bras Leader

# Accorder les autorisations au port

Donnez à tous les utilisateurs la permission de lire et d'écrire sur ces périphériques série

```Shell
sudo chmod 666 /dev/ttyACM*
```
