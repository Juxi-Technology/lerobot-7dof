[English](../en/10-inference/inference-rdk-s100.md) | [简体中文](../zh-hans/10-inference/inference-rdk-s100.md) | [繁體中文](../zh-hant/10-inference/inference-rdk-s100.md) | [Deutsch](../de/10-inference/inference-rdk-s100.md) | [Español](../es/10-inference/inference-rdk-s100.md) | Français | [Italiano](../it/10-inference/inference-rdk-s100.md) | [日本語](../ja/10-inference/inference-rdk-s100.md) | [한국어](../ko/10-inference/inference-rdk-s100.md) | [Português (BR)](../pt-br/10-inference/inference-rdk-s100.md) | [Português (PT)](../pt-pt/10-inference/inference-rdk-s100.md)

# Inférence sur D-Robotics RDK S100

Pour le déroulé détaillé de la mise en œuvre, consultez ce lien<cite doc-id="HSr8dBdZ0oQ5OwxPQvBcsuyZnWe" file-type="docx" title="Documentation complète du flux de travail LeRobot ACT Policy" type="doc"></cite>



## Déploiement de bout en bout du modèle ACT sur RDK S100/S100P

Cette section vous guide à travers la boucle de déploiement complète du modèle ACT sur le matériel D-Robotics RDK S100. L'ensemble du processus se divise en trois étapes clés : **l'export du modèle**, **la compilation de quantification** et **l'exécution à bord**.

<callout emoji="💡">
**Prérequis :**
- **Machine de développement (Host) :** sert à exécuter les étapes 1 et 2, il s'agit généralement de votre machine d'entraînement de modèles (elle doit être assez performante et disposer de Docker).
- **Carte (Edge) :** la D-Robotics RDK S100/S100P, sert à exécuter l'étape 3.
- **Chaîne d'outils :** cet article s'appuie sur le dépôt `rdk_LeRobot_tools` ; voir le [dépôt GitHub](https://github.com/D-Robotics/rdk_LeRobot_tools) pour plus de détails.
</callout>

<callout emoji="🚨">
**Note importante sur la compatibilité des versions (à lire absolument) :** le flux d'export ONNX actuel de `rdk_LeRobot_tools` est parfaitement compatible avec les **jeux de données LeRobot v2.1**. Comme la dernière version v3.0 modifie la structure des données, il est **fortement recommandé**, avant d'effectuer les opérations de cette section, de basculer le dépôt principal `lerobot` sur le commit spécifique compatible avec la v2.1, afin que le flux d'export se déroule sans accroc. 
*Commit ID recommandé :* `8cfab3882480bdde38e42d93a9752de5ed42cae2`
</callout>



### Étape 1 : exporter le modèle au format ONNX 💻 (sur la machine de développement)

D'abord, nous devons exporter le modèle **entraîné avec PyTorch** vers un format intermédiaire (ONNX).



#### **1. Cloner le dépôt de la chaîne d'outils** 

Placez-vous dans votre répertoire de travail `lerobot` et clonez la chaîne d'outils spécifique au RDK :

```Bash
cd lerobot

# 1. Basculer vers la version stable compatible avec les jeux de données v2.1
git checkout 8cfab3882480bdde38e42d93a9752de5ed42cae2

# 2. Cloner la chaîne d'outils spécifique au RDK de D-Robotics
git clone https://github.com/D-Robotics/rdk_LeRobot_tools.git
```



#### **2. Configurer les paramètres d'export** 

Modifiez le fichier `rdk_LeRobot_tools/bpu_export_config.yaml` et ajustez la configuration pour qu'elle corresponde à vos chemins réels :

```YAML
dataset:
  root: "data/so101_pick_place" # chemin absolu ou relatif vers votre jeu de données
act_path: "outputs/train/act_so101/checkpoints/050000/pretrained_model" # chemin vers les poids du modèle PyTorch d'origine
type: "nash-e" # architecture matérielle cible ; RDK S100 correspond à nash-e / S100P correspond à nash-m
```



#### 3. Exécuter le script d'export

```Bash
# Exporter en ONNX (machine de développement)
python export_bpu_actpolicy.py --config bpu_export_config.yaml
```

✅ **Indicateur de succès** : un dossier `bpu_export_output` est créé dans le répertoire courant, contenant le script `build_all.sh` et les données de calibration de quantification nécessaires par la suite.



### Étape 2 : compiler le modèle BPU 🐳 (dans un environnement Docker sur la machine de développement)

La quantification et la compilation des modèles BPU D-Robotics nécessitent un environnement OpenExplorer (OE). Nous recommandons d'utiliser Docker pour isoler l'environnement.



#### **1.** **Préparer l'environnement Docker et l'image** 

Assurez-vous que Docker est installé sur la machine de développement ([guide d'installation officiel](https://docs.docker.com/engine/install/)). Téléchargez l'image CPU recommandée et chargez-la :

```Bash
# Charger l'archive d'image hors ligne téléchargée
sudo docker load -i ai_toolchain_ubuntu_22_s100_xxx.tar
```



#### **2. Démarrer le conteneur de compilation**

<callout emoji="⚠️">
**Mise en garde** : la compilation du modèle nécessite une grande quantité de mémoire partagée. Veillez à ajouter l'argument `--shm-size=15g`, sinon des erreurs de mémoire IPC sont très probables.
</callout>

Montez le répertoire de travail de la machine de développement (contenant le dossier que vous venez d'exporter) dans le conteneur :

```Bash
sudo docker run -it --rm \
  --network host \
  --shm-size=15g \
  -v "$(pwd)":/workspace \
  --workdir /workspace \
  <docker-image-name> /bin/bash
```

(Remarque : remplacez `<docker-image-name>` par le nom réel de l'image que vous voyez via `sudo docker images`.)



#### **3.** **Lancer la compilation à l'intérieur du conteneur** 

Une fois à l'intérieur du conteneur, lancez le script de compilation en un clic :

```Bash
cd /workspace/bpu_export_output
bash build_all.sh
```



#### **4.** **Vérifier les artefacts de build** 

Une fois la compilation terminée, un dossier `bpu_output/` est créé sous `bpu_export_output`. Il contient tous les fichiers essentiels nécessaires à l'exécution sur la carte RDK : 

- Cliquez pour voir la structure du répertoire `bpu_output/`

  - `BPU_ACTPolicy_TransformerLayers.hbm` (fichier de modèle quantifié)
  - `BPU_ACTPolicy_VisionEncoder.hbm` (fichier de modèle quantifié)
  - `action_mean.npy` et plusieurs autres paramètres de normalisation du jeu de données
  - `camera1_mean.npy` et d'autres paramètres statistiques de la caméra

---

### Étape 3 : déploiement à bord et inférence 🤖 (sur le RDK S100)

<callout emoji="📌">
**Vérification des prérequis :**
1. La carte RDK dispose déjà de l'environnement d'exécution `D-Robotics/lerobot` configuré, avec `hbm_runtime` installé.
2. L'intégralité du dossier `bpu_output/` généré à l'étape précédente a été copiée sur la carte RDK, via `scp`, une clé USB ou un moyen similaire.
3. La configuration de base de la téléopération est déjà effectuée, en s'assurant que le port série du bras, le port USB de la caméra et le fichier de calibration sont correctement configurés.
</callout>



#### **1.** **Lancer l'inférence accélérée par BPU**

Dans le terminal de la carte RDK, placez-vous dans le répertoire de la chaîne d'outils et démarrez le script de contrôle :

```Bash
cd rdk_LeRobot_tools

python bpu_control_robot.py \
  --bpu-act-path ../bpu_output \
  --fps 30 \
  --inference-time 60
```



---

### 🛠️ Dépannage

Si vous rencontrez des problèmes lors d'un déploiement réel, reportez-vous à la liste suivante :

- **Le bras ne bouge pas ?**

  - Vérifiez que le périphérique est monté : tapez `ls /dev/ttyACM*` dans le terminal et confirmez que le port série du bras est correct.
  - Vérifiez les permissions : essayez d'exécuter le script d'inférence avec `sudo`, ou ajoutez l'utilisateur courant au groupe `dialout`.
- **Erreur de flux caméra / image anormale / le bras tremble sur place ?**

  - Vérifiez si l'index de la caméra a changé à cause d'un branchement à chaud, et contrôlez que les paramètres de la caméra dans le code correspondent bien au `/dev/video*` réel.
- **La copie des fichiers générés par le conteneur sur la machine de développement signale des « permissions insuffisantes » ?**

  - Les fichiers créés dans un répertoire monté par Docker appartiennent à root par défaut ; exécutez `sudo chown -R $USER:$USER bpu_export_output` sur la machine de développement pour corriger cela.
