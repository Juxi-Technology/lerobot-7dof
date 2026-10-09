[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | Français | [Italiano](../it/02-replace-files-for-7dof.md) | [日本語](../ja/02-replace-files-for-7dof.md) | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# Étape 2 : remplacer les fichiers (pour le 7-DOF)

## 1. Si vous avez cloné le dépôt de code officiel, quels fichiers remplacer/modifier

### Option A : utiliser directement le code de ce dépôt (recommandé)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Option B : remplacer manuellement après avoir cloné le lerobot officiel

Copiez **3 fichiers** de ce dépôt par-dessus votre clone officiel :

| Fichier de ce dépôt (source) | Écraser dans (cible) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | Le fichier du même nom dans le clone officiel |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | Le fichier du même nom dans le clone officiel |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> Prérequis : votre clone officiel a la même structure que la base de référence de ce dépôt (la version lerobot 2026-09).
> 
> Si les versions diffèrent beaucoup, **n'écrasez pas les fichiers entiers** ; effectuez plutôt les deux modifications manuelles ci-dessous.

### Modification manuelle lorsque les versions diffèrent (deux endroits seulement)

**① Dictionnaire des moteurs** (une copie dans `so_follower.py` et une dans `so_leader.py`, contenu identique) — remplacez

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

par

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # servomoteur nouvellement ajouté, rotation gauche/droite
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # le moteur de roulis n° 5 d'origine, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② Logique d'étalonnage** (la fonction `calibrate()` de chacun des deux fichiers) — supprimez le cas particulier de l'« articulation à tour complet » : remplacez

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

par une seule ligne, afin d'enregistrer la plage réelle de toutes les articulations :

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> Pourquoi : le code d'origine code en dur `wrist_roll` (roulis autour de l'axe de l'avant-bras) comme une articulation pouvant tourner sur un tour complet (0\~4095) et fixe sa plage sur toute l'étendue. Après la conversion en 7-DOF, les deux articulations du poignet (yaw / roll, servomoteurs 5/6) sont **toutes deux limitées mécaniquement et ne peuvent pas effectuer un tour complet** ; forcer une plage de tour complet pousse le code à envoyer des commandes d'articulation à des angles que la mécanique ne peut pas atteindre, avec un risque d'endommagement. Désormais, lors de l'étalonnage, le min/max réel de chaque moteur est enregistré manuellement.

### Remarque sur les bras Follower doubles

`bi_so_follower` / `bi_so_leader` (`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`) ne font qu'encapsuler un bras simple en ajoutant un préfixe `left_`/`right_` ; ils **ne contiennent aucune définition de moteur**. Tant que **les fichiers du bras simple ci-dessus** sont modifiés, les commandes pour bras double (`--robot.type=bi_so_follower`) sont automatiquement en 7-DOF.
