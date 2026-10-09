[English](../en/10-inference/infer-smolvla.md) | [简体中文](../zh-hans/10-inference/infer-smolvla.md) | [繁體中文](../zh-hant/10-inference/infer-smolvla.md) | [Deutsch](../de/10-inference/infer-smolvla.md) | [Español](../es/10-inference/infer-smolvla.md) | [Français](../fr/10-inference/infer-smolvla.md) | [Italiano](../it/10-inference/infer-smolvla.md) | [日本語](../ja/10-inference/infer-smolvla.md) | 한국어 | [Português (BR)](../pt-br/10-inference/infer-smolvla.md) | [Português (PT)](../pt-pt/10-inference/infer-smolvla.md)

# 추론 명령줄 - smolvla

## Ubuntu

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo chmod 666 /dev/ttyACM*
sudo rm -rf /home/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
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
  --policy.path=/home/tommy/Downloads/lerobot_output/shake/smolvla/40K/pretrained_model
```

## Mac

- 기존의 eval 접두사가 붙은 데이터셋 삭제(있는 경우)

```Shell
sudo rm -rf /Users/tommy/.cache/huggingface/lerobot/Tommymy/eval_lerobot_my_dataset_shake_hands
```

- 추론 명령줄

```Shell
lerobot-record  \
  --dataset.repo_id=Tommymy/eval_lerobot_my_dataset_shake_hands \
  --dataset.single_task="Shake Hands" \
  --policy.path=/Users/tommy/Downloads/7-lerobot/shake/smolvla/40K/pretrained_model \
  --dataset.push_to_hub=false \
  --robot.type=so101_follower \
  --robot.id=my_follower_arm \
  --robot.port=/dev/tty.usbmodem5AAF2193061 \
  --robot.cameras="{front: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 60, fourcc: "MJPG"}}" \
  --display_data=false \
  --dataset.episode_time_s=2000
```

<grid>
<column width-ratio="0.425772">
![이 이미지는 Ubuntu 환경에서의 추론 명령줄 인터페이스를 보여줍니다. 상단에는 캐시 사용, Delta Joint Actions Aloha 사용 같은 파라미터 설정을 포함해 실행 중인 명령이 표시되어 있습니다. 아래에는 로봇 id "zihao_follower_arm", 최대 상대 목표 None, 포트 "/dev/tty.usbmodemSAAF2193661", VLM 레이어 수가 16으로 줄었다는 안내 등 여러 핵심 정보가 있습니다. 이 이미지는 Ubuntu 추론 명령줄과 관련되어, 인터페이스와 몇 가지 핵심 파라미터 설정을 제시합니다.](../../en/images/d50-01.png)
</column>
<column width-ratio="0.574228">
![이 이미지는 Ubuntu 환경에서의 추론 명령줄 세션 중 터미널을 보여줍니다. 상단에는 calibration_dir, cameras 같은 로봇 관련 구성이 표시되어 있습니다. 아래에는 config.json, processor_config.json 등 여러 json 파일의 로딩 진행 막대가 있어 로딩 비율과 크기를 보여줍니다. 하단에는 "Mismatch between calibration values in the motor and the calibration file or no calibration file found" 같은 로그 메시지가 있어 모터의 캘리브레이션 값과 캘리브레이션 파일 간의 불일치를 알립니다. 이 이미지는 Ubuntu 추론 명령줄에 대응해, 실행 중의 터미널 피드백을 보여줍니다.](../../en/images/d50-02.png)
</column>
</grid>

## 결과

<grid><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768642205336.mp4](../../en/images/wx_camera_1768642205336.mp4)</figure></column><column width-ratio="0.500000"><figure view-type="Preview">[Attachment: wx_camera_1768574631300.mp4](../../en/images/wx_camera_1768574631300.mp4)</figure></column></grid>
