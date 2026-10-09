[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | Français | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | [한국어](../ko/03-find-serial-port/mac.md) | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Ordinateur Mac

## Vérifier le port

```Shell
ls /dev/tty.*
```

Le résultat ressemble à l'image ci-dessous ; l'un ou l'autre des deux ports fonctionnera

![L'image montre la sortie dans le terminal Mac après l'exécution de la commande « ls /dev/tty.* », listant plusieurs ports. Deux d'entre eux, « /dev/tty.usbmodem5AAF2194741 » et « /dev/tty.wchusbserial5AAF2194741 », sont mis en évidence par des cadres rouges. Le contexte mentionne la vérification des ports, et le résultat est similaire à cette image ; l'un ou l'autre des deux ports peut être utilisé. L'image présente visuellement les informations de port à vérifier, en lien étroit avec le contenu « Vérifier le port » ci-dessus, et constitue un affichage du résultat de l'opération de vérification des ports.](../../en/images/d09-01.png)

## Accorder les autorisations au port

Donnez à tous les utilisateurs la permission de lire et d'écrire sur ces périphériques série

```Shell
chmod 666 /dev/tty.*
```

## Noter mes ports

Bras Follower :

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Bras Leader :

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Pourquoi y a-t-il deux ports sur un Mac ?

La carte contrôleur de servomoteurs que nous utilisons est **reconnue par macOS comme deux types de pilotes série différents en même temps**, c'est pourquoi deux ports sont affichés :

- L'un est le pilote série générique par défaut du système (`/dev/tty.usbmodemxxxx`)
- L'autre est le pilote série dédié fourni par le fabricant de la puce (par exemple, « wch » ici désigne la puce CH340/CH341 de Nanjing Qinheng) (`/dev/tty.wchusbserialxxxx`)

C'est normal — **les deux ports correspondent en réalité au même périphérique matériel**, et vous pouvez vous connecter et communiquer via l'un ou l'autre (par exemple, choisissez simplement l'un des deux ports dans le logiciel qui pilote le bras robotisé).

Si vous rencontrez une erreur en utilisant un port plus tard, essayez de passer à l'autre.
