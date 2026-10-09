English | [简体中文](../zh-hans/10-inference/cli-reference.md) | [繁體中文](../zh-hant/10-inference/cli-reference.md) | [Deutsch](../de/10-inference/cli-reference.md) | [Español](../es/10-inference/cli-reference.md) | [Français](../fr/10-inference/cli-reference.md) | [Italiano](../it/10-inference/cli-reference.md) | [日本語](../ja/10-inference/cli-reference.md) | [한국어](../ko/10-inference/cli-reference.md) | [Português (BR)](../pt-br/10-inference/cli-reference.md) | [Português (PT)](../pt-pt/10-inference/cli-reference.md)

# Command Line Reference

## Command Line Notes

With live visualisation: --display_data=true

Without live visualisation: --display_data=false

With `--display_data=true`, the cool rerun.io visualisation interface is launched, but in the directory `/Users/tommy/.cache/huggingface/lerobot/eval_lerobot_my_dataset_a/images/observation.images.front/episode-000000` an image is saved for every frame, which takes up a lot of space. You can set it to `--display_data=false` later.



Infer a model from a HuggingFace model repo: --policy.path=Tommymy/lerobot_my_model_a



## Using the Grab Oranges Task as an Example

- Infer a local model (with live visualisation)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Infer a local model (without live visualisation)

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=false \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --dataset.episode_time_s=1000 \
  --policy.path=/Users/tommy/Downloads/7-lerobot/checkpoints/last/pretrained_model
```

- Infer a model from a HuggingFace model repo

```Shell
lerobot-record  \
  --robot.type=so101_follower \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --robot.id=my_follower_arm \
  --display_data=true \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_a \
  --dataset.single_task="Grab Oranges" \
  --policy.path=Tommymy/lerobot_my_model_a
```

The model is downloaded after running

![This image shows the interface for running the `pretrained_model.py` script on the command line. At the top it shows the model parameter configuration, such as `--display_data=true` and `--policy.path=TommyZihao/lerobot_zihao_model_a`. Below are parameter settings such as "robot", "camera" and "calibration_dir". At the bottom it shows the model download progress, currently at 68%. The image relates to the description of running the `pretrained_model.py` script and its parameters, visually presenting the parameter settings and download progress.](../../en/images/d48-01.png)
