[English](../en/09-train-model/train-act.md) | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | Français | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# Ligne de commande d'entraînement - ACT (recommandé pour les débutants)

## Documentation de référence

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## Pourquoi commencer par l'algorithme ACT

ACT est le premier modèle d'entraînement le plus recommandé lorsque l'on débute avec LeRobot. Ses atouts sont les suivants :

- Le modèle est très léger, avec seulement 80 millions de paramètres entraînables
- L'entraînement converge rapidement, et l'inférence est elle aussi rapide
- Vous obtenez des résultats après seulement une heure d'entraînement sur un seul GPU
- L'archive du modèle ACT fait environ 200 MB, ce qui la rend facile à stocker et à transférer
- Collecter environ 30 épisodes de données suffit généralement
- Il peut être déployé pour l'inférence sur un hôte Ubuntu, un Mac, un PC Windows, et même un Raspberry Pi
- L'inférence sur un robot réel fonctionne plutôt bien, et est largement suffisante pour des tâches simples comme saisir, serrer la main et poser un stylo
- L'algorithme ACT est déjà intégré à l'environnement de base de LeRobot, donc aucune bibliothèque supplémentaire n'est nécessaire

## Ligne de commande

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
  --dataset.root=~/lerobot_my_dataset_shake_hands \
  --dataset.revision=v0.1.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=~/output_lerobot_train/shake/act/ \
  --job_name=shake_act_a \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=20000 \
  --batch_size=8
```

## Notes sur la ligne de commande

Un `\` de continuation de ligne ne peut avoir qu'une seule espace avant lui et aucune espace après

Les paramètres affichés en rouge doivent être vérifiés ou modifiés avant chaque exécution

| Paramètre de ligne de commande | Description |
|-|-|
| --dataset.repo_id | Repo_ID du jeu de données HuggingFace |
| --dataset.root | Chemin local vers le jeu de données |
| --dataset.revision | Version du jeu de données, spécifiée lors du téléversement du jeu de données vers HuggingFace |
| --dataset.streaming | Le jeu de données est local, ce paramètre doit donc valoir `false`, car les données sont déjà sur le disque et aucune lecture en flux n'est nécessaire |
| --dataset.split | Vaut `train` par défaut, ce qui signifie que l'intégralité du jeu de données est utilisée comme ensemble d'entraînement |
| --policy.type | L'algorithme à entraîner, tel que act, smolvla, diffusion, pi0, wallx |
| --output_dir | Répertoire où la structure de sortie est enregistrée |
| --job_name | Nom de cette tâche d'entraînement |
| --policy.device | Périphérique de calcul |
| --wandb.enable | Activer la visualisation wandb |
| --wandb.project | Nom du projet wandb |
| --policy.push_to_hub | Pousser le modèle entraîné vers HuggingFace |
| --steps | Nombre de steps d'entraînement |
| --batch_size | Quantité de données fournie par step ; réduisez-la si vous manquez de mémoire GPU |
|  |  |

## Processus d'entraînement

<grid>
<column width-ratio="0.357753">
![Cette image montre un exemple d'utilisation de la commande d'entraînement `lerobot-train` depuis la ligne de commande. La commande définit plusieurs paramètres tels que `--dataset.repo_id` et `--dataset.root` pour spécifier les détails du jeu de données, définit `--policy.type` sur `act` et `--output_dir` sur le répertoire de sortie `outputs/lerobot_train/output_a`, ainsi que d'autres paramètres tels que `--job_name` et `--policy.device`. Elle liste également les valeurs par défaut de paramètres comme `--dataset.split` et `--policy.push_to_hub`. L'image est étroitement liée au contexte, montrant visuellement comment les paramètres de la commande d'entraînement sont définis.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![Cette image montre la sortie de la ligne de commande pendant l'entraînement. Elle affiche des détails d'entraînement du modèle tels que les réglages de scheduler, steps et use_policy_training_preset, ainsi que des paramètres liés au jeu de données. Elle montre aussi des informations telles que le nombre de paramètres du modèle et la loss, par exemple un num_total_params de 55917096 (52M) et une loss de 0.626. En dessous figurent des informations de téléchargement de fichiers, comme le téléchargement de « https://download.pytorch.org/models/resnet18-f37072fd.pth » vers le répertoire /home/featurize/.cache/torch/hub/checkpoints. L'image se rapporte à la ligne de commande d'entraînement décrite dans le contexte, montrant visuellement ce que la ligne de commande affiche pendant l'entraînement.](../../en/images/d36-02.png)
</column>
</grid>

![Cette image montre les informations de journal produites pendant l'entraînement. Le journal enregistre plusieurs steps d'entraînement par ordre chronologique, incluant l'heure, le nombre d'itérations d'entraînement, la loss et le taux d'apprentissage — par exemple, le 14 janvier 2024 à 15:11:53, le nombre d'itérations était de 131k et la loss de 0.368. Ici `INFO` est le type de journal, `train` la phase d'entraînement, `step` le nombre d'itérations, `loss` la valeur de loss et `lr` le taux d'apprentissage. L'image se rapporte à la section des notes sur la ligne de commande du document, présentant visuellement les données clés de la session d'entraînement.](../../en/images/d36-03.png)

L'archive du modèle fait environ 300 MB
