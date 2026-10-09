[English](../en/10-inference/infer-pi0.md) | 简体中文 | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | [한국어](../ko/10-inference/infer-pi0.md) | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# 推理命令行-pi0

## Ubuntu

- 删除原有的eval开头的数据集（如有）

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- 推理命令行

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
![图片展示的是在Ubuntu系统下使用SSH连接到机器时出现的错误信息。画面中显示了控制台输出的代码错误，指出平台不被支持，无法获取X连接，还提示需确保有运行的X服务器，且DISPLAY环境变量设置正确。此外，还显示了关于无头环境的警告信息，以及录制第0集的记录。该图片与文档中Ubuntu推理命令行操作上下文相关，可能是操作过程中遇到的异常情况。](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![图片展示的是在Ubuntu系统下运行推理命令行时的输出结果。命令执行过程中，出现多次“E0119”错误提示，指出在自动调优时无有效triton配置，且out of resources，如shared memory资源不足。还显示了多个triton_msm模型的运行参数，如ALLOW_TF32、BLOCK_K、BLOCK_M等，以及相应的ACC_TYPE、ALLOW_TF32、BLOCK_K、BLOCK_M等信息。该图片与文档中Ubuntu推理命令行操作上下文相关，展示了运行过程中遇到的资源不足问题。](../../en/images/d51-02.png)
</column>
</grid>

![图片展示的是在Ubuntu系统下推理命令行操作的终端界面。界面中显示了多个triton_mm指令的执行结果，如triton_mm_3644耗时0.2355ms等，均采用t1.float32类型，ALLOW_TF32=True，BLOCK_K等参数也有所显示。最后显示SingleProcess AUTOTUNE benchmarking耗时0.7305秒和0.0001秒预编译20个选择。该图片与文档中Ubuntu推理命令行操作上下文相关，展示了具体执行情况。](../../en/images/d51-03.png)

> **视频待补**：原文此处嵌有 `VID_20260120_182109.mp4`（原始 310MB）。飞书侧该文件未提供可下载的视频流，只有元数据，因此未能抓取。需查看请到[原文档](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg)。



## Mac

- 删除原有的eval开头的数据集（如有）

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 推理命令行

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
![图片展示的是在Ubuntu系统下使用11 - yolo26推理命令行时的终端界面。界面中显示了Python 3.12版本信息，以及对robot - type设置为follower的记录。还列出了摄像头相关参数，如color_mode、fourcc、fps、height、width等。此外，还显示了加载模型的路径，以及一些警告信息，如模型加载错误等。该图片与文档中Ubuntu推理命令行操作上下文相关，直观呈现了操作过程中的终端反馈信息。](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![图片展示的是在Ubuntu系统中使用Python代码进行推理时的命令行输出。输出中包含多个信息，如“PIBPytorch model”加载成功，“WARNING”提示可能需要处理的模型键，“INFO”显示OpenCV摄像头连接成功等。此外，还出现了多次“huggingface/tokenizers: The process current just got forked...”的警告信息，提示因fork操作导致的并行问题。该图片与上下文介绍的Ubuntu系统推理命令行操作相关，展示了实际运行时可能出现的各类信息及警告。](../../en/images/d51-05.png)
</column>
</grid>

## 用Mac推理，机械臂一顿一顿的原因

- 数据集太小了
- 显卡显存不够，需要上50系显卡