English | [简体中文](../zh-hans/09-train-model/train-pi05.md) | [繁體中文](../zh-hant/09-train-model/train-pi05.md) | [Deutsch](../de/09-train-model/train-pi05.md) | [Español](../es/09-train-model/train-pi05.md) | [Français](../fr/09-train-model/train-pi05.md) | [Italiano](../it/09-train-model/train-pi05.md) | [日本語](../ja/09-train-model/train-pi05.md) | [한국어](../ko/09-train-model/train-pi05.md) | [Português (BR)](../pt-br/09-train-model/train-pi05.md) | [Português (PT)](../pt-pt/09-train-model/train-pi05.md)

# Training Command Line - pi0.5

## Reference Documentation

https://github.com/huggingface/lerobot/blob/46e19ae579f80ce66211afafd1c3c649c569131f/docs/source/pi0.mdx

https://www.pi.website/blog/pi05

## Recommended Cloud GPU Instance

![This image shows the details of an RTX A6000 cloud GPU instance offered by Alibaba Cloud. Its pay-as-you-go price is 3.29 CNY/hour, and 3 cards are available. The instance is configured with an RTX A6000 GPU with a total of 51.0 GB of GPU memory, a 30-core AMD EPYC 7742 CPU, 60.9 GB of memory and 429.5 GB of disk. There is a blue "Start Using" button at the bottom. The image relates to the "Recommended Cloud GPU Instance" section, visually presenting the recommended cloud GPU configuration, price and other key information.](../../en/images/d40-01.png)

## Install the Environment

```Shell
cd lerobot
pip install -e ".[pi0]"
```

## Command Line

- Delete the files under output left by the previous interrupted training run

```Shell
sudo rm -rf output_lerobot_train/shake/pi05_A
```

- Train

```Shell
lerobot-train \
    --dataset.repo_id=Tommymy/lerobot_my_dataset_shake_hands \
    --dataset.root=~/lerobot_my_dataset_shake_hands \
    --dataset.revision=v0.1.0 \
    --policy.type=pi05 \
    --output_dir=~/output_lerobot_train/shake/pi05_A \
    --job_name=shake_pi05_A \
    --policy.pretrained_path=lerobot/pi05_base \
    --policy.compile_model=true \
    --policy.gradient_checkpointing=true \
    --policy.dtype=bfloat16 \
    --policy.freeze_vision_encoder=false \
    --policy.train_expert_only=false \
    --steps=50000 \
    --policy.device=cuda \
    --policy.push_to_hub=false \
    --wandb.enable=true \
    --wandb.project=Lerobot_my_Project \
    --batch_size=8
```

<grid>
<column width-ratio="0.468128">
![This image shows the output when training a physical-intelligence (PI) model from the command line. It shows the model being loaded, parameters being remapped, and the optimizer and scheduler being created, for example "Loading model from: lerobot/pi05_base". Warning messages also appear, such as "Warning: Could not remap state dicts: ['loading'] in state_dict for PolicyPolicy". It also presents training-related figures such as "num_total_frames: 180K". The image relates to the training command-line operation, visually presenting the key information from the training run.](../../en/images/fix-03.png)
</column>
<column width-ratio="0.531872">
![This image shows the output during command-line training. During training the huggingface/torch process is forked; since parallelism is already in use, parallelism is disabled to avoid a deadlock, with a message advising to avoid doing this "before the fork if possible". Training only properly begins after 20 minutes. The image is closely related to the context, presenting the process operations and advisory messages that can appear during training and helping to explain the state and things to watch out for.](../../en/images/d40-02.png)
</column>
</grid>

After the command line has been running for 20 minutes, training only then properly begins

The model archive is about 5 GB, and about 7 GB once extracted
