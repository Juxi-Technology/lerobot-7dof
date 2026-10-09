[English](../en/10-inference/infer-pi0.md) | [简体中文](../zh-hans/10-inference/infer-pi0.md) | [繁體中文](../zh-hant/10-inference/infer-pi0.md) | [Deutsch](../de/10-inference/infer-pi0.md) | [Español](../es/10-inference/infer-pi0.md) | [Français](../fr/10-inference/infer-pi0.md) | [Italiano](../it/10-inference/infer-pi0.md) | [日本語](../ja/10-inference/infer-pi0.md) | 한국어 | [Português (BR)](../pt-br/10-inference/infer-pi0.md) | [Português (PT)](../pt-pt/10-inference/infer-pi0.md)

# 추론 명령줄 - pi0

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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/pi0/50K/pretrained_model
```

<grid>
<column width-ratio="0.494604">
![이 이미지는 Ubuntu 환경에서 SSH로 머신에 연결할 때 나타나는 오류를 보여줍니다. 플랫폼이 지원되지 않으며 X 연결을 설정할 수 없다는 콘솔 오류가 표시되고, X 서버가 실행 중인지 그리고 DISPLAY 환경 변수가 올바르게 설정되어 있는지 확인하라고 안내합니다. 또한 헤드리스 환경에 대한 경고와 에피소드 0이 녹화되었다는 기록도 보여줍니다. 이 이미지는 Ubuntu 추론 명령줄과 관련되어, 실행 중 마주칠 수 있는 비정상 상황일 수 있습니다.](../../en/images/d51-01.png)
</column>
<column width-ratio="0.505396">
![이 이미지는 Ubuntu 환경에서 추론 명령줄을 실행할 때의 출력을 보여줍니다. 실행 중 "E0119" 오류 메시지가 여러 번 나타나며, 오토튜닝 중 유효한 triton 구성이 없고 공유 메모리 부족처럼 리소스가 고갈되었다고 알립니다. 또한 ALLOW_TF32, BLOCK_K, BLOCK_M 등 여러 triton_mm 모델의 런타임 파라미터와 그에 대응하는 ACC_TYPE, ALLOW_TF32, BLOCK_K, BLOCK_M 값도 보여줍니다. 이 이미지는 Ubuntu 추론 명령줄과 관련되어, 실행 중 마주친 리소스 부족을 보여줍니다.](../../en/images/d51-02.png)
</column>
</grid>

![이 이미지는 Ubuntu 환경에서 추론 명령줄 세션 중의 터미널을 보여줍니다. 예를 들어 triton_mm_3644가 0.2355 ms 걸린 여러 triton_mm 명령의 결과가 표시되며, 모두 t1.float32 타입에 ALLOW_TF32=True를 사용하고 BLOCK_K 같은 파라미터도 보여줍니다. 마지막에는 SingleProcess AUTOTUNE 벤치마킹에 0.7305초, 20개 선택지를 사전 컴파일하는 데 0.0001초가 걸렸다고 표시됩니다. 이 이미지는 Ubuntu 추론 명령줄과 관련되어, 실제 실행을 보여줍니다.](../../en/images/d51-03.png)

> **영상 대기 중**: 원문은 이 위치에 `VID_20260120_182109.mp4`(원래 310 MB)를 삽입했습니다. Feishu 측에서 이 파일에 대해 다운로드 가능한 영상 스트림을 제공하지 않고 메타데이터만 제공해 수집할 수 없었습니다. 보려면 [원문 문서](https://juxitech.feishu.cn/wiki/NOWXw9NOJiDTs2kRr7RcdIrKnvg)를 참조하세요.



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
![이 이미지는 Ubuntu 환경에서의 추론 명령줄 세션(11 - yolo26) 중 터미널을 보여줍니다. Python 3.12 버전 정보와 robot-type이 follower로 설정되었다는 기록이 표시됩니다. color_mode, fourcc, fps, height, width 등 카메라 관련 파라미터도 나열되고, 모델을 로드한 경로와 모델 로딩 오류 같은 일부 경고 메시지도 보여줍니다. 이 이미지는 Ubuntu 추론 명령줄과 관련되어, 실행 중의 터미널 피드백을 제시합니다.](../../en/images/d51-04.png)
</column>
<column width-ratio="0.534183">
![이 이미지는 Ubuntu 환경에서 Python 코드로 추론을 실행할 때의 명령줄 출력을 보여줍니다. "PIBPytorch model"이 성공적으로 로드되었다는 내용, 처리해야 할 수 있는 모델 키에 대한 "WARNING", OpenCV 카메라가 성공적으로 연결되었다는 "INFO" 등 여러 정보가 담겨 있습니다. 또한 포크로 인한 병렬 처리 문제를 알리는 "huggingface/tokenizers: The process current just got forked..." 경고가 여러 번 표시됩니다. 이 이미지는 문맥에서 설명한 Ubuntu 추론 명령줄과 관련되어, 런타임에 나타날 수 있는 여러 메시지와 경고를 보여줍니다.](../../en/images/d51-05.png)
</column>
</grid>

## Mac에서 추론하면 팔이 떨리는 이유

- 데이터셋이 너무 작습니다
- GPU 메모리가 충분하지 않습니다. 50 시리즈 카드가 필요합니다
