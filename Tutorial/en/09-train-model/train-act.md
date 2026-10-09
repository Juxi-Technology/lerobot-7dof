English | [简体中文](../zh-hans/09-train-model/train-act.md) | [繁體中文](../zh-hant/09-train-model/train-act.md) | [Deutsch](../de/09-train-model/train-act.md) | [Español](../es/09-train-model/train-act.md) | [Français](../fr/09-train-model/train-act.md) | [Italiano](../it/09-train-model/train-act.md) | [日本語](../ja/09-train-model/train-act.md) | [한국어](../ko/09-train-model/train-act.md) | [Português (BR)](../pt-br/09-train-model/train-act.md) | [Português (PT)](../pt-pt/09-train-model/train-act.md)

# Training Command Line - ACT (Recommended for Beginners)

## Reference Documentation

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/act.mdx

https://github.com/huggingface/lerobot/blob/main/src/lerobot/scripts/lerobot_train.py

https://github.com/huggingface/lerobot/blob/main/src/lerobot/configs/train.py

## Why Start with the ACT Algorithm

ACT is the most recommended first model to train when you get into LeRobot. Its advantages are:

- The model is very lightweight, with only 80 million learnable parameters
- Training converges quickly, and inference is fast too
- You can see results after just one hour of training on a single GPU
- The ACT model archive is about 200 MB, making it easy to store and transfer
- Collecting about 30 episodes of data is usually enough
- It can be deployed for inference on an Ubuntu host, a Mac, a Windows PC, even a Raspberry Pi
- Inference on a real robot works quite well, and is more than sufficient for simple tasks such as picking, handshaking and pen placing
- The ACT algorithm is already built into LeRobot's base environment, so no extra libraries are needed

## Command Line

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

## Command Line Notes

A line-continuation `\` may only have one space before it and no space after it

Parameters shown in red must be checked or changed before every run

| Command-line parameter | Description |
|-|-|
| --dataset.repo_id | Repo_ID of the HuggingFace dataset |
| --dataset.root | Local path to the dataset |
| --dataset.revision | Dataset version, specified when you uploaded the dataset to HuggingFace |
| --dataset.streaming | The dataset is local, so this must be `false`, since the data is already on disk and no streaming read is needed |
| --dataset.split | Defaults to `train`, meaning the full dataset is used as the training set |
| --policy.type | The algorithm to train, such as act, smolvla, diffusion, pi0, wallx |
| --output_dir | Directory where the output structure is saved |
| --job_name | Name of this training job |
| --policy.device | Compute device |
| --wandb.enable | Enable wandb visualisation |
| --wandb.project | wandb project name |
| --policy.push_to_hub | Push the trained model to HuggingFace |
| --steps | Number of training steps |
| --batch_size | Amount of data fed in per step; lower it if you run out of GPU memory |
|  |  |

## Training Process

<grid>
<column width-ratio="0.357753">
![This image shows an example of using the `lerobot-train` training command from the command line. The command sets several parameters such as `--dataset.repo_id` and `--dataset.root` to specify dataset details, sets `--policy.type` to `act` and `--output_dir` to the output directory `outputs/lerobot_train/output_a`, along with other parameters such as `--job_name` and `--policy.device`. It also lists the default values of parameters such as `--dataset.split` and `--policy.push_to_hub`. The image is closely related to the context, visually showing how the parameters of the training command are set.](../../en/images/d36-01.png)
</column>
<column width-ratio="0.642247">
![This image shows the command-line output during training. It displays model training details such as the settings for scheduler, steps and use_policy_training_preset, along with dataset-related parameters. It also shows information such as the number of model parameters and the loss, for example num_total_params of 55917096 (52M) and a loss of 0.626. Below that is file download information, such as downloading "https://download.pytorch.org/models/resnet18-f37072fd.pth" to the /home/featurize/.cache/torch/hub/checkpoints directory. The image relates to the training command line described in the context, visually showing what the command line outputs during training.](../../en/images/d36-02.png)
</column>
</grid>

![This image shows the log information produced during training. The log records several training steps in chronological order, including the time, training iteration count, loss and learning rate — for example, at 15:11:53 on 14 January 2024 the iteration count was 131k and the loss was 0.368. Here `INFO` is the log type, `train` the training phase, `step` the iteration count, `loss` the loss value and `lr` the learning rate. The image relates to the command-line notes section in the document, visually presenting the key data from the training run.](../../en/images/d36-03.png)

The model archive is about 300 MB
