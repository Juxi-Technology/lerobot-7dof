[English](../en/09-train-model/cloud-gpu-setup.md) | [简体中文](../zh-hans/09-train-model/cloud-gpu-setup.md) | [繁體中文](../zh-hant/09-train-model/cloud-gpu-setup.md) | [Deutsch](../de/09-train-model/cloud-gpu-setup.md) | [Español](../es/09-train-model/cloud-gpu-setup.md) | [Français](../fr/09-train-model/cloud-gpu-setup.md) | [Italiano](../it/09-train-model/cloud-gpu-setup.md) | [日本語](../ja/09-train-model/cloud-gpu-setup.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/cloud-gpu-setup.md) | [Português (PT)](../pt-pt/09-train-model/cloud-gpu-setup.md)

# 클라우드 GPU 학습 환경 구성

## 사용하는 컴퓨터의 네트워크 프록시 끄기

그렇지 않으면 Jupyter 명령줄을 열지 못할 수 있습니다

## 클라우드 GPU 플랫폼 Featurize 로그인

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

사용자 그룹에 가입한 뒤, 상담원에게 자신이 퉁지쯔하오(同济子豪兄)의 팬이라고 말하면 쿠폰을 받을 수 있습니다

## 클라우드 GPU 인스턴스 시작

## 환경 설치 및 구성

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

## wandb 로그인

```Shell
wandb login
API Key를 복사해 붙여 넣고 Enter를 누르세요
```

![이 이미지는 LeRobot 프로젝트의 wandb 로그인 화면으로, 로그인 과정의 정보를 기록하고 있습니다. wandb 로그인을 시작하며 사용자에게 지정된 주소에서 API 키를 찾아 키를 붙여 넣고 Enter를 눌러 제출하라고 안내합니다. 또한 netrc 파일을 찾지 못했고 해당 netrc 파일 경로에 API 키를 추가하고 있다는 내용을 보여주며, 이후 로그인이 완료되어 현재 로그인 사용자가 tommyzihao로 표시되고 강제 재로그인 명령도 함께 제공됩니다. 이 이미지는 문서의 "wandb 로그인" 단계에 대응해 로그인 과정과 결과를 보여줍니다.](../../en/images/d35-01.png)

## 데이터셋 마운트

```Shell
인스턴스 다운로드 명령을 복사합니다. 예를 들면:
featurize dataset download 7f40bdaa-b1a4-4c00-9652-ff26fd079109

unzip lerobot_zihao_dataset_shake_hands.zip
```

데이터셋은 `~` 디렉터리 아래에 나타납니다

## 가중치 저장 주기 변경(선택)

`lerobot/src/lerobot/configs/train.py`를 엽니다

save_freq를 20_000에서 5_000으로 변경합니다

이렇게 하면 학습 중 더 이른 시점에 모델 가중치 파일을 얻을 수 있습니다
