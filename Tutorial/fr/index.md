[English](../en/index.md) | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | [Español](../es/index.md) | Français | [Italiano](../it/index.md) | [日本語](../ja/index.md) | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>Tutoriel SO-ARM101 7-DOF</title>

# SO-ARM101 7-DOF · Préparation avant exécution

> Ce guide s'adresse aux personnes qui ont **converti un SO-ARM101 de 6 servomoteurs à 7 servomoteurs** et qui souhaitent maintenant exécuter l'ensemble du flux de travail LeRobot (étalonnage → enregistrement → entraînement → déploiement) sur ce bras.  
> Code correspondant : ce dépôt (`lerobot-7dof`), un fork du lerobot officiel qui ne modifie que la configuration des moteurs liés au SO.

---

## 0. Vérifiez d'abord votre bras robotique

7 servomoteurs (tous des STS3215). Correspondance entre les ID de servomoteur et les articulations :

| **ID du servomoteur** | **Nom de l'articulation** | **Description** |
|-|-|-|
| 1 | `shoulder_pan` | Rotation horizontale de l'épaule |
| 2 | `shoulder_lift` | Levée de l'épaule |
| 3 | `elbow_flex` | Flexion du coude |
| 4 | `wrist_flex` | Tangage du poignet (flexion vers le haut/bas) |
| 5 | `wrist_yaw` | Lacet du poignet (rotation gauche/droite d'environ 90°) · **le servomoteur nouvellement ajouté cette fois** (inséré entre les servomoteurs 4 et 5 d'origine) |
| 6 | `wrist_roll` | Roulis du poignet · à l'origine le moteur de roulis n° 5, ID 5→6 ; la pièce imprimée est inchangée et le nom reste identique |
| 7 | `gripper` | Pince · à l'origine ID=6, décalé à 7 après la modification |

Ordre des données d'articulation (l'ordre des dimensions d'articulation de `action` / `observation.state` dans les fichiers Parquet après l'enregistrement) :  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`.

⚠️ Remarque : **les données, les fichiers d'étalonnage et les modèles déjà entraînés de la version à 6 servomoteurs sont incompatibles avec ce dépôt**, vous devez donc tout refaire ci-dessous.

---

## 1. Si vous avez cloné le dépôt de code officiel, quels fichiers remplacer/modifier

### Option A : utiliser directement le code de ce dépôt (recommandé)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### Option B : remplacer manuellement après avoir cloné le lerobot officiel

Copiez **3 fichiers** de ce dépôt par-dessus votre clone officiel :

| Fichier de ce dépôt (source) | Écraser dans (cible) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | Le fichier du même nom dans le clone officiel |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | Le fichier du même nom dans le clone officiel |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | Le fichier du même nom dans le clone officiel (**commentaires uniquement**, aucun impact fonctionnel, facultatif) |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <clone-officiel>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <clone-officiel>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> Prérequis : votre clone officiel a la même structure que la base de référence de ce dépôt (la version lerobot 2026-08). Si les versions diffèrent beaucoup, **n'écrasez pas les fichiers entiers** ; effectuez plutôt les deux modifications manuelles ci-dessous.

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
