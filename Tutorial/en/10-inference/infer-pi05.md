English | [简体中文](../zh-hans/10-inference/infer-pi05.md) | [繁體中文](../zh-hant/10-inference/infer-pi05.md) | [Deutsch](../de/10-inference/infer-pi05.md) | [Español](../es/10-inference/infer-pi05.md) | [Français](../fr/10-inference/infer-pi05.md) | [Italiano](../it/10-inference/infer-pi05.md) | [日本語](../ja/10-inference/infer-pi05.md) | [한국어](../ko/10-inference/infer-pi05.md) | [Português (BR)](../pt-br/10-inference/infer-pi05.md) | [Português (PT)](../pt-pt/10-inference/infer-pi05.md)

# Inference Command Line - pi0.5

## Ubuntu

- Delete the existing eval-prefixed dataset (if any)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- Inference command line

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi05/50K/pretrained_model
```















## Mac

- Delete the existing eval-prefixed dataset (if any)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inference command line

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
![This image shows the command-line interface for controlling a robot with Python 3.12 and the mujoco simulation environment in an Ubuntu environment. It shows part of the code, along with warnings and error messages that appear during the run: addCriterion](../../en/images/d52-01.png)
</column>
<column width-ratio="0.534183">
![This image shows the output from running related code with Python 3.7.12 and PyTorch 1.12.0 in an Ubuntu environment. It contains several warnings and messages at the "WARNING" and "INFO" levels.](../../en/images/d52-02.png)
</column>
</grid>

## Why Inference Is Slow

- The dataset is too small
- The GPU doesn't have enough memory; you need a 50-series card
