English | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | [한국어](../ko/09-train-model/cloud-gpu-setup.md) | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# Configuring a Cloud GPU Training Environment

## Disable Your Computer's Network Proxy

Otherwise you may not be able to open the Jupyter command line

## Log In to the Cloud GPU Platform Featurize

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

Join the user group, tell customer service you are a fan of Zihao from Tongji, and get a coupon

## Start a Cloud GPU Instance

## Install and Configure the Environment

```Shell
conda create -y -n lerobot python=3.12
conda activate lerobot
conda install ffmpeg=7.1.1 -c conda-forge -y
# git clone https://github.com/Seeed-Projects/lerobot.git ~/work/Lerobot
git clone https://github.com/huggingface/lerobot.git
cd lerobot
pip install -e ".[pi]"
pip install wandb --upgrade
# export HF_ENDPOINT=https://hf-mirror.com
hf auth login
```

## Log In to wandb

```Shell
wandb login
Copy and paste the API Key, then press Enter
```

![The image shows the process of logging in to W&B (Weights & Biases) using the `wandb login` command in the LeRobot environment. After the command runs, W&B prompts you to log in at https://api.wandb.ai, says you can deploy a local W&B server, and guides you to find the API key. Then W&B prompts you to paste the API key and press Enter, and also notes that no .netrc file was found and is being created. Finally W&B shows that you are logged in as tommyzihao, with the login status https://api.wandb.ai. This image relates to the "Log In to wandb" step in the document, visually presenting the feedback from the login operation.](../../en/images/d35-01.png)

## Mount the Dataset

```Shell
Copy the instance download command, something like:
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

The dataset appears under the `~` directory

## Adjust the Weight Save Frequency (Optional)

Open `lerobot/src/lerobot/configs/train.py`

Change save_freq from 20_000 to 5_000

This way you get model weight files earlier in training
