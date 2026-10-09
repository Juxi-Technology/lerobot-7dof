English | [简体中文](../zh-hans/09-train-model/local-ubuntu-training.md) | [繁體中文](../zh-hant/09-train-model/local-ubuntu-training.md) | [Deutsch](../de/09-train-model/local-ubuntu-training.md) | [Español](../es/09-train-model/local-ubuntu-training.md) | [Français](../fr/09-train-model/local-ubuntu-training.md) | [Italiano](../it/09-train-model/local-ubuntu-training.md) | [日本語](../ja/09-train-model/local-ubuntu-training.md) | [한국어](../ko/09-train-model/local-ubuntu-training.md) | [Português (BR)](../pt-br/09-train-model/local-ubuntu-training.md) | [Português (PT)](../pt-pt/09-train-model/local-ubuntu-training.md)

# Local Ubuntu Training

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

- Note

`\` may only have one space before it, and no space after it

`--dataset.split` defaults to `train`, meaning the full dataset is used as the training set

The dataset is local, so `--dataset.streaming` must be `false`, since the dataset is already on disk and no streaming read is needed

```Shell
lerobot-train \
  --dataset.repo_id=Tommymy/lerobot_my_dataset_a \
  --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a \
  --dataset.revision=v0.4.0 \
  --dataset.streaming=false \
  --policy.type=act \
  --output_dir=output_lerobot_train/a \
  --job_name=orange_job \
  --policy.device=cuda \
  --wandb.enable=true \
  --wandb.project=Lerobot_my_Project \
  --policy.push_to_hub=false \
  --steps=300000 \
  --batch_size=8
  
lerobot-train --dataset.repo_id=Tommymy/lerobot_my_dataset_a --dataset.root=/Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a --dataset.revision=v0.4.0 --dataset.streaming=false --policy.type=act --output_dir=output_lerobot_train/a --job_name=orange_job --policy.device=cuda --wandb.enable=true --wandb.project=Lerobot_my_Project --policy.push_to_hub=false --steps=300000 --batch_size=8
```

<grid>
<column width-ratio="0.357753">
![This image shows the training command and configuration for running the lerobot_train.py script in a local Ubuntu environment. The command includes parameters such as the dataset path, repository ID and branch, for example `--dataset.repo_id` set to Tommy/lerobot_zhao_dataset_a. Among the configuration values, `--dataset.streaming` is set to `false`, `--use_imagenet_stats` to `True`, `--batch_size` to 4 and `--val_n_episodes` to 1000. The image is closely related to the context, visually presenting the training command and its key configuration parameters.](../../en/images/d34-01.png)
</column>
<column width-ratio="0.642247">
![This image shows the training log produced by running the lerobot_train.py script in a local Ubuntu environment, centred on the training configuration parameters and the live status of the training run. It clearly marks key training information: `--dataset.split` defaults to `train`, meaning the full dataset is used as the training set, and because the dataset is stored locally the state of `--dataset.streaming` is likewise fixed. The log also covers training progress, dataset loading, the creation of the model optimizer and scheduler, and the loss and step figures during training, giving a clear view of a local training run in progress.](../../en/images/d34-02.png)
</column>
</grid>

![This image shows the training log produced when training a model with LeRobot in an Ubuntu environment. The log records information from the training run, such as the time, training set, model, loss and accuracy. The timestamps run from 15:11:53 to 16:15:16 on 14 January 2024, the loss fluctuates between 0.68 and 0.65, and the accuracy (acc) between 0.85 and 0.88. The image relates to the LeRobot model training context, visually presenting how the key metrics change during training.](../../en/images/d34-03.png)
