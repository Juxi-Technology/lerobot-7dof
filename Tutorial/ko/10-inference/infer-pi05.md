[English](../en/10-inference/infer-pi05.md) | [简体中文](../zh-hans/10-inference/infer-pi05.md) | [繁體中文](../zh-hant/10-inference/infer-pi05.md) | [Deutsch](../de/10-inference/infer-pi05.md) | [Español](../es/10-inference/infer-pi05.md) | [Français](../fr/10-inference/infer-pi05.md) | [Italiano](../it/10-inference/infer-pi05.md) | [日本語](../ja/10-inference/infer-pi05.md) | 한국어 | [Português (BR)](../pt-br/10-inference/infer-pi05.md) | [Português (PT)](../pt-pt/10-inference/infer-pi05.md)

# 추론 명령줄 - pi0.5

## Ubuntu

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
export TOKENIZERS_PARALLELISM=false
```

- 추론 명령줄

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

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 추론 명령줄

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
![이 이미지는 Ubuntu 환경에서 Python 3.12와 mujoco 시뮬레이션 환경으로 로봇을 제어하는 명령줄 인터페이스를 보여줍니다. 코드의 일부와 함께 실행 중 나타나는 경고 및 오류 메시지, 예를 들어 addCriterion이 표시되어 있습니다.](../../en/images/d52-01.png)
</column>
<column width-ratio="0.534183">
![이 이미지는 Ubuntu 환경에서 Python 3.7.12와 PyTorch 1.12.0으로 관련 코드를 실행한 출력을 보여줍니다. "WARNING"과 "INFO" 수준의 여러 경고와 메시지가 담겨 있습니다.](../../en/images/d52-02.png)
</column>
</grid>

## 추론이 느린 이유

- 데이터셋이 너무 작습니다
- GPU 메모리가 충분하지 않습니다. 50 시리즈 카드가 필요합니다
