[English](../en/so-arm101-dual-arm.md) | [简体中文](../zh-hans/so-arm101-dual-arm.md) | [繁體中文](../zh-hant/so-arm101-dual-arm.md) | [Deutsch](../de/so-arm101-dual-arm.md) | [Español](../es/so-arm101-dual-arm.md) | Français | [Italiano](../it/so-arm101-dual-arm.md) | [日本語](../ja/so-arm101-dual-arm.md) | [한국어](../ko/so-arm101-dual-arm.md) | [Português (BR)](../pt-br/so-arm101-dual-arm.md) | [Português (PT)](../pt-pt/so-arm101-dual-arm.md)

# Tutoriel du bras double SO-ARM101

## Introduction

Ce guide détaille le flux de travail complet pour entraîner un système robotique SO-ARM à deux bras avec LeRobot, y compris le câblage matériel, l'étalonnage des deux bras, la téléopération à deux bras, l'enregistrement et la gestion du jeu de données, l'entraînement d'une politique ACT et le déploiement sur le robot réel. En suivant ce guide, vous pouvez utiliser deux bras Leader et deux bras Follower pour collecter des données de démonstration, entraîner une politique d'apprentissage par imitation et l'exécuter sur les bras réels.

Commencez par tout câbler comme suit

| Rôle | Port |
|-|-|
| Follower gauche | /dev/ttyACM0 |
| Follower droit | /dev/ttyACM1 |
| Leader gauche | /dev/ttyACM2 |
| Leader droit | /dev/ttyACM3 |

Le type de follower est so101_follower et le type de leader est so101_leader (dans LeRobot, so100_leader et so101_leader partagent la même implémentation).

## Prérequis

### 0.1 Installer les dépendances

Pour la configuration de l'environnement, reportez-vous au tutoriel SO-ARM :

### 0.2 Autorisations USB

```Bash
sudo chmod 666 /dev/ttyACM0 /dev/ttyACM1 /dev/ttyACM2 /dev/ttyACM3
```

## Étalonnage (étape critique)

### 1.1 Étalonner le follower gauche

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.id=my_so101_bi_follower_left
```

### 1.2 Étalonner le follower droit

```Bash
lerobot-calibrate \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower_right
```

### 1.3 Étalonner le leader gauche

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM2 \
  --teleop.id=my_so101_bi_leader_left
```

### 1.4 Étalonner le leader droit

```Bash
lerobot-calibrate \
  --teleop.type=so101_leader \
  --teleop.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader_right
```

Après l'étalonnage, les fichiers sont enregistrés dans :

```Bash
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_left.json
~/.cache/huggingface/lerobot/calibration/robots/so_follower/my_so101_bi_follower_right.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_left.json
~/.cache/huggingface/lerobot/calibration/teleoperators/so_leader/my_so101_bi_leader_right.json
```

> Remarque sur les noms de répertoires : so101_follower et so100_follower, ainsi que so101_leader et so100_leader, partagent la même implémentation ; les répertoires sont donc unifiés en so_follower / so_leader. Le leader est un téléopérateur, ses fichiers d'étalonnage se trouvent donc sous teleoperators/ et non sous robots/.

### (Facultatif) Si vous avez étalonné auparavant avec d'autres ID

Par exemple, si vous avez utilisé auparavant my_awesome_follower_arm1, my_awesome_follower_arm2, etc., vous pouvez copier les fichiers d'étalonnage :

```Bash
CAL_DIR=~/.cache/huggingface/lerobot/calibration

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm1.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_left.json

cp $CAL_DIR/robots/so_follower/my_awesome_follower_arm2.json \
   $CAL_DIR/robots/so_follower/my_so101_bi_follower_right.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm3.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_left.json

cp $CAL_DIR/teleoperators/so_leader/my_awesome_leader_arm4.json \
   $CAL_DIR/teleoperators/so_leader/my_so101_bi_leader_right.json
```

---

## Téléopération à deux bras

### 2.1 Sans caméras

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### 2.2 Avec caméras

Vous pouvez utiliser lerobot-find-cameras opencv pour vérifier les indices de caméras, et ajouter ou retirer des caméras à volonté.

```Bash
lerobot-teleoperate \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --display_data=true
```

### Conseils de sécurité

- Surveillez les alentours et évitez les collisions entre les bras Follower.

## Enregistrement d'un jeu de données

### 3.1 Enregistrement local (sans téléversement vers le Hub)

Ajoutez --dataset.root (le répertoire dans lequel les données sont écrites) et --dataset.push_to_hub=false, et ajoutez --dataset.no_stamp=true pour garder un nom de jeu de données stable (sinon un horodatage est automatiquement ajouté au repo_id, et la reprise / relecture / entraînement ultérieurs ne le retrouveront plus).

> Remarque : le repo_id doit contenir un / (sous la forme nom-utilisateur/nom-du-jeu-de-données) ; un jeu de données local n'est pas réellement téléversé.

```Bash
lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> L'encodage vidéo est déjà libsvtav1 par défaut, il n'a donc pas besoin d'être précisé ; pour le personnaliser, utilisez un paramètre imbriqué tel que --dataset.rgb_encoder.vcodec=h264.

Les données sont enregistrées sous ./datasets/bi_so101_task/, avec cette structure :

```Bash
├── meta/
│   ├── info.json         # Informations du jeu de données (fps, formes des features, etc.)
│   ├── episodes/         # Métadonnées par épisode (chunk-000/...)
│   ├── stats.json        # Statistiques de normalisation pour chaque feature
│   └── tasks.parquet     # Texte de la tâche → task_index
├── data/                 # Données de features image par image (chunk-*.parquet)
└── videos/               # Un sous-répertoire par caméra (chunk-*.mp4)
```

### 3.2 Téléversement vers le Hugging Face Hub

Si vous souhaitez un téléversement automatique, conservez HF_USER et retirez root et push_to_hub=false (le téléversement est la valeur par défaut). Gardez les ports et les indices de caméras cohérents avec le tableau de câblage :

```Bash
export HF_USER=your_hf_username

lerobot-record \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=50 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

> Le nom du dépôt Hub téléversé est ${HF_USER}/bi_so101_task, correspondant au repo_id utilisé pour l'entraînement via le Hub en 4.2 ci-dessous. Une copie locale est d'abord enregistrée dans ~/.cache/huggingface/lerobot/${HF_USER}/bi_so101_task/.

### 3.3 Poursuivre l'enregistrement (reprise)

Si l'enregistrement s'est arrêté de façon inattendue (par exemple, vous avez quitté par un clic droit pendant la phase de réinitialisation), ou si vous voulez terminer la collecte en plusieurs sessions, utilisez --resume pour continuer à ajouter des épisodes au même jeu de données.

**Remarque** :

- Vous devez ajouter --resume=true, sinon LeRobotDataset.create() renvoie une erreur car le répertoire existe déjà.
- Dans la commande de reprise, --dataset.root et --dataset.repo_id doivent correspondre exactement au premier enregistrement (3.1) (la reprise exige un root explicite).
- --dataset.num_episodes est **le nombre d'épisodes à enregistrer cette fois**, et non le total visé. Par exemple, si vous en avez déjà enregistré 15 et que vous en voulez 50 au total, écrivez 35.
- À la sortie, essayez de quitter pendant l'enregistrement d'un épisode ou juste après qu'il se termine naturellement ; évitez de quitter pendant la phase « Reset the environment » (cela fait échouer l'enregistrement d'un épisode vide).

```Bash
lerobot-record \
  --resume=true \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --teleop.type=bi_so_leader \
  --teleop.left_arm_config.port=/dev/ttyACM2 \
  --teleop.right_arm_config.port=/dev/ttyACM3 \
  --teleop.id=my_so101_bi_leader \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.push_to_hub=false \
  --dataset.no_stamp=true \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.num_episodes=35 \
  --dataset.fps=30 \
  --dataset.episode_time_s=30 \
  --dataset.reset_time_s=10 \
  --dataset.video=true \
  --display_data=true
```

### 3.4 Relecture et suppression d'épisodes

#### Relire un épisode précis

```Bash
lerobot-replay \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --dataset.episode=24
```

> episode est un indice commençant à 0, donc 24 désigne le 25e épisode.

#### Supprimer un épisode précis

```Bash
python -m lerobot.scripts.lerobot_edit_dataset \
  --repo_id=juxi/bi_so101_task \
  --root=./datasets/bi_so101_task \
  --operation.type=delete_episodes \
  --operation.episode_indices="[24]"
```

La suppression réécrit le jeu de données sur place, et les données d'origine sont sauvegardées dans ./datasets/bi_so101_task_old/. Une fois que vous avez confirmé que le nouveau jeu de données est correct, vous pouvez supprimer manuellement la sauvegarde :

```Bash
rm -rf ./datasets/bi_so101_task_old
```

#### Supprimer tout le jeu de données

```Bash
rm -rf ./datasets/bi_so101_task
```

---

## Entraînement ACT

### 4.1 Entraîner à partir d'un jeu de données local

```Bash
lerobot-train \
  --dataset.repo_id=juxi/bi_so101_task \
  --dataset.root=./datasets/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=60000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> --dataset.root pointe vers le répertoire du jeu de données enregistré en 3.1 (le repo_id doit correspondre à celui utilisé lors de l'enregistrement). Si le répertoire --output_dir existe déjà, une FileExistsError est levée immédiatement — utilisez un nouveau répertoire de sortie ou ajoutez --resume=true pour poursuivre l'entraînement.

### 4.2 Entraîner à partir du Hugging Face Hub

```Bash
export HF_USER=your_hf_username

lerobot-train \
  --dataset.repo_id=${HF_USER}/bi_so101_task \
  --policy.type=act \
  --policy.device=cuda \
  --steps=100000 \
  --output_dir=outputs/train/act_bi_so101 \
  --wandb.enable=false \
  --policy.push_to_hub=false
```

> La commande ci-dessus utilise les paramètres par défaut d'ACT (chunk_size=100, dim_model=512, etc.).
> 
> Le repo_id doit correspondre au nom du dépôt utilisé lors du téléversement en 3.2 (3.2 ajoute --dataset.no_stamp=true, le nom du dépôt est donc fixé à \${HF_USER}/bi_so101_task). Aucun --dataset.root n'est nécessaire pour l'entraînement ; il est téléchargé automatiquement depuis le Hub.

## Déploiement sur le robot réel

> Remarque : lerobot-record sert uniquement à collecter des données de démonstration. Utilisez lerobot-rollout pour déployer une politique entraînée — la version actuelle de lerobot-record n'accepte plus --policy.path et rejette aussi les noms de jeux de données au préfixe eval\_ .

### 5.1 Évaluation sur site (aucune donnée enregistrée)

```Bash
lerobot-rollout \
  --strategy.type=base \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --task="Pick the cube with left arm and hand it to right arm" \
  --duration=60 \
  --display_data=true
```

- --duration est le nombre de secondes d'exécution ; 0 signifie aucune limite de temps.
- Pour prendre la main / arrêter en cours d'exécution, ajoutez --interactive=true et utilisez des commandes telles que /stop et /reset dans le terminal.

### 5.2 Évaluer et enregistrer des données (en local)

Utilisez la stratégie épisodique (elle se comporte comme l'ancien lerobot-record : enregistre par épisode avec une phase de réinitialisation) :

```Bash
lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=juxi/rollout_bi_so101_task \
  --dataset.root=./datasets/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

> Un nom de jeu de données de déploiement doit commencer par rollout\_ (exigence stricte de la version actuelle). Lors d'un enregistrement local, ajoutez --dataset.root et --dataset.no_stamp=true pour éviter qu'un horodatage soit ajouté au nom du répertoire.

### 5.3 Téléverser les données d'évaluation vers le Hugging Face Hub

```Bash
export HF_USER=your_hf_username

lerobot-rollout \
  --strategy.type=episodic \
  --policy.path=outputs/train/act_bi_so101/checkpoints/last/pretrained_model \
  --robot.type=bi_so_follower \
  --robot.left_arm_config.port=/dev/ttyACM0 \
  --robot.right_arm_config.port=/dev/ttyACM1 \
  --robot.id=my_so101_bi_follower \
  --robot.left_arm_config.cameras='{
    left_wrist: {"type": "opencv", "index_or_path": 2, "width": 640, "height": 480, "fps": 30}
  }' \
  --robot.right_arm_config.cameras='{
    right_wrist: {"type": "opencv", "index_or_path": 4, "width": 640, "height": 480, "fps": 30}
  }' \
  --dataset.repo_id=${HF_USER}/rollout_bi_so101_task \
  --dataset.no_stamp=true \
  --dataset.num_episodes=10 \
  --dataset.single_task="Pick the cube with left arm and hand it to right arm" \
  --dataset.fps=30 \
  --display_data=true
```

## FAQ

| Problème | Cause | Solution |
|-|-|-|
| La téléopération demande de réétalonner | bi_so_follower ne trouve pas les fichiers d'étalonnage avec le suffixe \_left / \_right | Réétalonnez avec des ID incluant _left / \_right, ou copiez les fichiers d'étalonnage existants |
| Le bras Leader ne peut pas être déplacé | Le couple du Leader n'est pas désactivé | Réétalonnez ou vérifiez le moteur |
| La reprise d'enregistrement signale que le répertoire existe déjà | --resume=true n'a pas été ajouté | Ajoutez --resume=true à la commande lerobot-record |
| --resume=true renvoie une erreur et exige un root | La reprise exige un répertoire de jeu de données explicite | Ajoutez --dataset.root=./datasets/bi_so101_task à la commande de reprise, en correspondance avec le premier enregistrement |
| Le nom du répertoire du jeu de données comporte un horodatage en trop, la relecture/l'entraînement ne le trouve pas | no_stamp n'a pas été défini à l'enregistrement, un horodatage a donc été ajouté au repo_id | Ajoutez --dataset.no_stamp=true lors de l'enregistrement/de la reprise |
| --dataset.vcodec=... signale que le paramètre n'existe pas | C'est un ancien paramètre ; le paramètre d'encodage vidéo est désormais imbriqué | Utilisez --dataset.rgb_encoder.vcodec=h264 à la place (la valeur par défaut est déjà libsvtav1) |
| Pendant le déploiement, lerobot-record signale une erreur --policy.path / eval\_ | La version actuelle de lerobot-record n'inclut plus le déploiement de politique | Utilisez lerobot-rollout --strategy.type=episodic pour le déploiement, avec des noms de jeux de données commençant par rollout_ |
| Les bras gauche et droit sont inversés | Mauvaise configuration des ports | Échangez left_arm_config.port et right_arm_config.port |
| L'entraînement ne trouve pas le jeu de données | Aucun root n'a été indiqué pour le jeu de données local | Ajoutez --dataset.root=./datasets/xxx lors de l'entraînement |
| Le jeu de données est téléversé automatiquement | push_to_hub=false n'a pas été défini | Ajoutez --dataset.push_to_hub=false lors de l'enregistrement |
| À la sortie, le message You must add one or several frames before calling add_episode apparaît | Vous avez quitté pendant la phase de réinitialisation, l'épisode courant n'a donc aucune image | N'affecte pas les données déjà enregistrées ; utilisez --resume=true pour poursuivre la collecte |
