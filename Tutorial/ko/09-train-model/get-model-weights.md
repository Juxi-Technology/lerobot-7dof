[English](../en/09-train-model/get-model-weights.md) | [简体中文](../zh-hans/09-train-model/get-model-weights.md) | [繁體中文](../zh-hant/09-train-model/get-model-weights.md) | [Deutsch](../de/09-train-model/get-model-weights.md) | [Español](../es/09-train-model/get-model-weights.md) | [Français](../fr/09-train-model/get-model-weights.md) | [Italiano](../it/09-train-model/get-model-weights.md) | [日本語](../ja/09-train-model/get-model-weights.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/get-model-weights.md) | [Português (PT)](../pt-pt/09-train-model/get-model-weights.md)

# 모델 가중치 파일 받기



```Shell
cd ~/output_lerobot_train/a/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```

모델 가중치는 20K step마다 한 번 저장됩니다.



마우스 오른쪽 버튼을 클릭해 `ckpt.zip` 압축 파일을 다운로드하고 로컬 컴퓨터에 압축을 풉니다








## 악수(Shake Hands)

```Shell
# 특정 학습 step에서 저장된 모델
# cd ~/output_lerobot_train/shake/diffusion/checkpoints/005000

# 최신 모델
cd ~/output_lerobot_train/shake/wallx/checkpoints/last
zip -r ~/ckpt.zip pretrained_model
```
