English | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# Inference Command Line - pi0

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![This image shows an error that appears when connecting to the machine over SSH in an Ubuntu environment. It shows a console error stating that the platform is not supported, that the X connection cannot be established, and advising you to make sure an X server is running and the DISPLAY environment variable is set correctly. It also shows a warning about a headless environment and a record of episode 0 being recorded. The image relates to the Ubuntu inference command line and may be an abnormal situation encountered during operation.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![This image shows the output when running the inference command line in an Ubuntu environment. During execution, "E0119" error messages appear several times, stating that there is no valid triton config during autotuning and that resources are exhausted, such as insufficient shared memory. It also shows the runtime parameters of several triton_mm models, such as ALLOW_TF32, BLOCK_K and BLOCK_M, along with the corresponding ACC_TYPE, ALLOW_TF32, BLOCK_K and BLOCK_M values. The image relates to the Ubuntu inference command line, showing a resource shortage encountered during the run.](../../en/images/d51-02.png)
</column>
</grid>

![This image shows the terminal during an inference command-line session in an Ubuntu environment. It shows the results of several triton_mm instructions, for example triton_mm_3644 taking 0.2355 ms, all using the t1.float32 type with ALLOW_TF32=True, and also shows parameters such as BLOCK_K. At the end it shows SingleProcess AUTOTUNE benchmarking taking 0.7305 seconds and 0.0001 seconds to precompile 20 choices. The image relates to the Ubuntu inference command line, showing the actual execution.](../../en/images/d51-03.png)

> **Video pending**: the original text embeds `VID_20260120_182109.mp4` (originally 310 MB) here. On the Feishu side no downloadable video stream was provided for this file, only metadata, so it could not be captured. To view it, see the [original document](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg).



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
![This image shows the terminal during an inference command-line session (11 - yolo26) in an Ubuntu environment. It shows Python 3.12 version information and a record of robot-type being set to follower. It also lists camera-related parameters such as color_mode, fourcc, fps, height and width, and shows the path from which the model is loaded along with some warning messages, such as model-loading errors. The image relates to the Ubuntu inference command line, presenting the terminal feedback during the operation.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![This image shows the command-line output from running inference with Python code in an Ubuntu environment. It contains several items of information, such as "PIBPytorch model" loading successfully, "WARNING" about model keys that may need to be handled, and "INFO" indicating that the OpenCV camera connected successfully. It also shows the warning "huggingface/tokenizers: The process current just got forked..." several times, flagging a parallelism problem caused by the fork. The image relates to the Ubuntu inference command line described in the context, showing the various messages and warnings that can appear at runtime.](../../en/images/d51-05.png)
</column>
</grid>

## Why Inference on a Mac Makes the Arm Stutter

- The dataset is too small
- The GPU doesn't have enough memory; you need a 50-series card
