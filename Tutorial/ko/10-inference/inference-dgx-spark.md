[English](../en/10-inference/inference-dgx-spark.md) | [简体中文](../zh-hans/10-inference/inference-dgx-spark.md) | [繁體中文](../zh-hant/10-inference/inference-dgx-spark.md) | [Deutsch](../de/10-inference/inference-dgx-spark.md) | [Español](../es/10-inference/inference-dgx-spark.md) | [Français](../fr/10-inference/inference-dgx-spark.md) | [Italiano](../it/10-inference/inference-dgx-spark.md) | [日本語](../ja/10-inference/inference-dgx-spark.md) | 한국어 | [Português (BR)](../pt-br/10-inference/inference-dgx-spark.md) | [Português (PT)](../pt-pt/10-inference/inference-dgx-spark.md)

# NVIDIA DGX Spark 추론

## 환경 설치

- PyTorch

공식 사이트에서 PyTorch를 별도로 설치하며, CUDA 13.0 버전을 사용합니다

```Shell
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
```

![이 이미지는 터미널에서 LeRobot 환경을 설치하는 명령과 결과를 보여줍니다. 먼저 "pip install -e /Downloads/lerobot" 명령을 실행하고, 이어서 "python -m lrobot -h"를 실행해 LeRobot 도움말 정보를 조회하며 LeRobot 버전이 0.4.4임을 표시합니다. 마지막으로 "pip show lrobot"을 실행해 LeRobot의 작성자, 홈페이지 등의 정보를 표시합니다. 이 이미지는 LeRobot 환경 설치와 관련되어, 설치 과정과 결과를 제시합니다.](../../en/images/d56-01.png)

- 그런 다음 pyproject.toml 파일에서 torch를 따로 주석 처리합니다

![이 이미지는 pyproject.toml 파일의 내용을 보여주며, torchcode="2.3.0, c2.8.0" 줄이 빨간 상자로 강조되어 있습니다. 이 파일은 프로젝트 의존성을 지정하는 데 사용되는 Python 프로젝트 구성 파일입니다. 문맥에서는 pyproject.toml 파일에서 torch를 따로 주석 처리한 뒤 pip install -e를 실행한다고 언급하며, 이미지는 그 문맥과 관련되어 다음 단계의 참고로 pyproject.toml 파일에서 torchcode의 위치를 시각적으로 제시합니다.](../../en/images/d56-02.png)

그런 다음 pip install -e . 를 실행합니다

- 주의: 추론 명령줄에서 policy.path 경로를 Spark 안의 실제 모델 경로로 변경하세요

## 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo chmod 666 /dev/ttyACM*
```



```Shell
sudo rm -rf /home/apx103/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

## ACT

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
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/ACT/5K/pretrained_model
```

## SmolVLA

- 환경 설치

```Shell
conda activate base
conda create -y -n lerobot-smolvla python=3.10 -y
conda activate lerobot-smolvla
conda install ffmpeg=7.1.1 -c conda-forge -y

pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130

cd lerobot
pip install -e ".[feetech,smolvla]"
```

- 추론

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
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## WALL-OSS

- 환경 설치

```Shell
cd lerobot
pip install -e ".[feetech,wallx]"
```

- 코드 추가

![이 이미지는 lerobot 프로젝트의 policies 폴더에 있는 factory.py 파일의 일부를 보여줍니다. 빨간 상자 안의 핵심 코드는 "from lerobot.policies.wall_x.processor_wall_x import make_wall_x_pre_post_processors"이며, "processors = make" 같은 구문도 함께 있습니다. 이 이미지는 pi0 모델 추론 절과 관련되어, pi0 추론 작업을 완료하려면 이 코드를 추가해야 함을 설명하며, pi0 추론 코드의 중요한 부분입니다.](../../en/images/d56-03.png)

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/wallx/30K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/ttyACM0 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=1000
```

## pi0

- 환경 설치

```Shell
conda create -y -n lerobot-pi python=3.10 -y
conda activate lerobot-pi
conda install ffmpeg=7.1.1 -c conda-forge -y

cd lerobot
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu130
pip install -e ".[feetech,pi]"
```

- 추론

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
  --policy.path=/home/apx103/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```
