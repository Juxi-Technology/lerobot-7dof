[English](../en/09-train-model/upload-model-to-huggingface.md) | [简体中文](../zh-hans/09-train-model/upload-model-to-huggingface.md) | [繁體中文](../zh-hant/09-train-model/upload-model-to-huggingface.md) | [Deutsch](../de/09-train-model/upload-model-to-huggingface.md) | [Español](../es/09-train-model/upload-model-to-huggingface.md) | [Français](../fr/09-train-model/upload-model-to-huggingface.md) | [Italiano](../it/09-train-model/upload-model-to-huggingface.md) | [日本語](../ja/09-train-model/upload-model-to-huggingface.md) | 한국어 | [Português (BR)](../pt-br/09-train-model/upload-model-to-huggingface.md) | [Português (PT)](../pt-pt/09-train-model/upload-model-to-huggingface.md)

# HuggingFace에 모델 업로드(선택)

## 모델 저장소 만들기

<grid>
<column width-ratio="0.354197">
![이 이미지는 HuggingFace 사용자 인터페이스를 보여줍니다. 아바타 아이콘이 있고, 이것을 클릭하면 드롭다운 메뉴가 열리며 그 안에서 "New Model" 옵션이 빨간 상자로 강조되어 있습니다. 이 이미지는 "HuggingFace에 모델 업로드(선택)" 절과 관련되어 "모델 저장소 만들기" 단계에 대응하며, HuggingFace에서 새 모델을 만드는 진입점을 시각적으로 제시해 사용자가 플랫폼에서 모델 관련 리소스를 만드는 방법을 이해하도록 돕습니다.](../../en/images/d46-01.png)
</column>
<column width-ratio="0.645803">
![이 이미지는 HuggingFace 웹사이트에서 새 모델 저장소를 만드는 인터페이스를 보여줍니다. "Owner" 드롭다운은 "TommyZihao"로, "Model name" 필드는 "lerobot_zihao_model_a"로, "License" 필드는 "mit"로 설정되어 있습니다. 아래에는 "Base template" 옵션과 "Public", "Private" 저장소 유형 선택이 있습니다. 이 이미지는 "모델 저장소 만들기" 절과 관련되어, 모델 저장소를 만들 때 정보를 입력하는 예시를 보여줍니다.](../../en/images/d46-02.png)
</column>
</grid>

## 모델 저장소 보기

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

지금은 비어 있습니다

![이 이미지는 HuggingFace 플랫폼의 "TommyZihao/lerobot_zihao_model_a" 모델 페이지를 보여줍니다. 왼쪽에는 모델 카드를 편집하는 "Model card" 탭이 있습니다. 오른쪽의 "Getting started with your model" 영역은 완전한 모델 정보 추가와 모델 파일 푸시를 포함해 모델 사용을 시작하는 방법을 설명합니다. 그 아래의 "Edit Model Card" 영역에서는 모델의 License, 언어, base model 등의 정보를 추가할 수 있습니다. 맨 아래 "Push your model files" 영역은 CLI, Python, Git, HTTPS, SSH 등 모델 파일을 업로드하는 여러 방법을 제공합니다. 이 이미지는 HuggingFace에 모델을 업로드하는 것과 관련되어, 페이지의 조작 요소를 보여줍니다.](../../en/images/d46-03.png)

![이 이미지는 HuggingFace 플랫폼의 TommyZihao/lerobot_zihao_model_a 모델 저장소 페이지를 보여줍니다. 페이지에는 모델의 파일 크기가 1.54 KB, 기여자 1명, 9분 전에 이루어진 커밋 1건의 이력이 표시되어 있습니다. 또한 .gitattributes와 README.md 파일이 각각 1.52 KB와 24 Bytes 크기로 나열되며, 이들 역시 최초 커밋에서 비롯된 것으로 9분 전의 것입니다. 이 이미지는 HuggingFace에 모델을 업로드하는 것과 관련되어, 모델 업로드 후 페이지가 어떻게 보이는지 보여줍니다.](../../en/images/d46-04.png)

## 모델 업로드

다음 내용으로 `upload_model.py` 파일을 만듭니다

```Python
from huggingface_hub import HfApi

api = HfApi()

repo_id = "TommyZihao/lerobot_zihao_model_shake_hands"

api.upload_folder(
    folder_path="~/output_lerobot_train/b/checkpoints/last/pretrained_model",
    repo_id=repo_id,
    repo_type="model"
)

api.create_tag(repo_id, tag="v0.1.0", repo_type="model")
```

실행합니다

```Shell
python upload_model.py
```

![이 이미지는 명령줄에서 `python upload_model.py` 명령을 실행한 출력을 보여줍니다. 파일 처리 진행률이 34%, 신규 데이터 업로드 진행률도 34%로 표시되며, `d_model/model.safetensors`와 `tokenizer_processor.safetensors` 두 파일의 업로드 진행률이 각각 92%로 나열되어 있습니다. 이 이미지는 HuggingFace에 모델을 업로드하는 것과 관련되어, 모델 파일을 업로드할 때의 진행 상황을 시각적으로 보여줍니다.](../../en/images/d46-05.png)

## 모델 저장소 보기

https://huggingface.co/TommyZihao/lerobot_zihao_model_a

![이 이미지는 TommyZihao의 lerobot_zihao_model_a 모델의 HuggingFace 저장소 페이지를 보여줍니다. 페이지에는 모델의 License가 mit, 기여자 1명, 커밋 이력 2건이 표시되어 있습니다. 가운데에는 README.md, config.json, model.safetensors 등 여러 파일이 나열되어 있고, 각 파일 오른쪽에 "Upload folder using huggingface_hub"라는 텍스트가 있어 이 파일들이 huggingface_hub를 통해 업로드되었음을 나타냅니다. 이 이미지는 HuggingFace에 모델을 업로드하는 것과 관련되어, 모델 파일이 HuggingFace에 어떻게 저장되는지 시각적으로 보여줍니다.](../../en/images/d46-06.png)

이제 모델 파일이 준비되었습니다
