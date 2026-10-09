English | [简体中文](../zh-hans/10-inference/infer-wall-oss.md) | [繁體中文](../zh-hant/10-inference/infer-wall-oss.md) | [Deutsch](../de/10-inference/infer-wall-oss.md) | [Español](../es/10-inference/infer-wall-oss.md) | [Français](../fr/10-inference/infer-wall-oss.md) | [Italiano](../it/10-inference/infer-wall-oss.md) | [日本語](../ja/10-inference/infer-wall-oss.md) | [한국어](../ko/10-inference/infer-wall-oss.md) | [Português (BR)](../pt-br/10-inference/infer-wall-oss.md) | [Português (PT)](../pt-pt/10-inference/infer-wall-oss.md)

# Inference Command Line - WALL-OSS

## Ubuntu

- Delete the existing eval-prefixed dataset (if any)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inference command line

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

![This image shows the terminal during an inference command-line session in an Ubuntu environment. It shows several parameter settings, such as a total video pixel count of 90316800 and a preprocessing config file size of 2.46 KB. It also shows the loading progress for files such as tokenizer.json and tokenizer_config.json, for example tokenizer.json loading at 100%. At the top there are "INFO" messages, and below are model-loading prompts such as "Loading model from:". The image corresponds to the Ubuntu inference command line, presenting the key information during the operation.](../../en/images/d53-01.png)

<figure view-type="Preview">[Attachment: c7b8a795e52ca10689d296d212dc8e53.mp4](../../en/images/c7b8a795e52ca10689d296d212dc8e53.mp4)</figure>





## Inference Command Line - Mac

- Delete the existing eval-prefixed dataset (if any)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- Inference command line

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

<figure view-type="Preview">[Attachment: 9f4567abdcce7f983415aef8f42877d0.mp4](../../en/images/9f4567abdcce7f983415aef8f42877d0.mp4)</figure>
