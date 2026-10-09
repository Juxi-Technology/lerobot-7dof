[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | Français | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# Ligne de commande d'inférence - pi0

## Ubuntu

- Supprimer le jeu de données existant préfixé par eval (le cas échéant)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- Ligne de commande d'inférence

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![Cette image montre une erreur qui apparaît lors de la connexion à la machine via SSH dans un environnement Ubuntu. Elle affiche une erreur de console indiquant que la plateforme n'est pas prise en charge, que la connexion X ne peut pas être établie, et conseille de s'assurer qu'un serveur X est en cours d'exécution et que la variable d'environnement DISPLAY est correctement définie. Elle montre aussi un avertissement sur un environnement headless et une trace de l'enregistrement de l'épisode 0. L'image se rapporte à la ligne de commande d'inférence Ubuntu et peut correspondre à une situation anormale rencontrée en cours d'exécution.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![Cette image montre la sortie produite par l'exécution de la ligne de commande d'inférence dans un environnement Ubuntu. Pendant l'exécution, des messages d'erreur « E0119 » apparaissent plusieurs fois, indiquant qu'il n'y a pas de configuration triton valide lors de l'autotuning et que les ressources sont épuisées, par exemple une mémoire partagée insuffisante. Elle montre aussi les paramètres d'exécution de plusieurs modèles triton_mm, tels que ALLOW_TF32, BLOCK_K et BLOCK_M, ainsi que les valeurs correspondantes ACC_TYPE, ALLOW_TF32, BLOCK_K et BLOCK_M. L'image se rapporte à la ligne de commande d'inférence Ubuntu, montrant une pénurie de ressources rencontrée pendant l'exécution.](../../en/images/d51-02.png)
</column>
</grid>

![Cette image montre le terminal pendant une session en ligne de commande d'inférence dans un environnement Ubuntu. Elle affiche les résultats de plusieurs instructions triton_mm, par exemple triton_mm_3644 prenant 0.2355 ms, toutes utilisant le type t1.float32 avec ALLOW_TF32=True, et montre aussi des paramètres tels que BLOCK_K. À la fin, elle affiche un benchmark SingleProcess AUTOTUNE prenant 0.7305 secondes et 0.0001 secondes pour précompiler 20 choix. L'image se rapporte à la ligne de commande d'inférence Ubuntu, montrant l'exécution réelle.](../../en/images/d51-03.png)

> **Vidéo à venir** : le texte d'origine intègre ici `VID_20260120_182109.mp4` (à l'origine 310 MB). Côté Feishu, aucun flux vidéo téléchargeable n'a été fourni pour ce fichier, seulement des métadonnées, il n'a donc pas pu être récupéré. Pour la consulter, voyez le [document d'origine](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg).



## Mac

- Supprimer le jeu de données existant préfixé par eval (le cas échéant)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Ligne de commande d'inférence

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --policy.freeze_vision_encoder=false \
  --policy.dtype=bfloat16 \
  --policy.compile_model=true \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --dataset.episode_time_s=1000 \
  --dataset.push_to_hub=false \
  --policy.device=cpu \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.465817">
![Cette image montre le terminal pendant une session en ligne de commande d'inférence (11 - yolo26) dans un environnement Ubuntu. Elle affiche des informations de version de Python 3.12 et une trace indiquant que robot-type a été défini sur follower. Elle liste aussi des paramètres liés à la caméra tels que color_mode, fourcc, fps, height et width, et montre le chemin depuis lequel le modèle est chargé ainsi que des messages d'avertissement, comme des erreurs de chargement du modèle. L'image se rapporte à la ligne de commande d'inférence Ubuntu, présentant le retour du terminal pendant l'opération.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![Cette image montre la sortie en ligne de commande produite par l'exécution de l'inférence avec du code Python dans un environnement Ubuntu. Elle contient plusieurs éléments d'information, tels que le chargement réussi du « PIBPytorch model », un « WARNING » sur des clés de modèle qui pourraient devoir être traitées, et un « INFO » indiquant que la caméra OpenCV s'est connectée avec succès. Elle montre aussi plusieurs fois l'avertissement « huggingface/tokenizers: The process current just got forked... », signalant un problème de parallélisme causé par le fork. L'image se rapporte à la ligne de commande d'inférence Ubuntu décrite dans le contexte, montrant les divers messages et avertissements qui peuvent apparaître à l'exécution.](../../en/images/d51-05.png)
</column>
</grid>

## Pourquoi l'inférence sur un Mac fait trembler le bras

- Le jeu de données est trop petit
- Le GPU n'a pas assez de mémoire ; il vous faut une carte de la série 50
