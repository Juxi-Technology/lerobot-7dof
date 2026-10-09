[English](../en/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [简体中文](../zh-hans/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [繁體中文](../zh-hant/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Deutsch](../de/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Español](../es/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Français](../fr/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Italiano](../it/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [日本語](../ja/07-collect-dataset-real/upload-dataset-to-huggingface.md) | 한국어 | [Português (BR)](../pt-br/07-collect-dataset-real/upload-dataset-to-huggingface.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/upload-dataset-to-huggingface.md)

<title>데이터셋을 HuggingFace에 업로드(선택 사항)</title>

# 방법 1: 로컬에서 업로드(권장하지 않음; 업로드 속도가 느림)

- 자동 업로드

데이터셋을 수집할 때 `push_to_hub=true`로 설정하면 수집이 끝난 뒤 자동으로 업로드됩니다

![이 이미지는 명령 줄 인터페이스로, 데이터셋 업로드 과정의 실행 로그 일부를 보여 줍니다. 상단에는 SVN, treet W2 같은 도구의 환경 정보가 표시되고, 가운데에는 "Starting the second pass: moving the mov atom to the beginning of the file" 같은 처리 메시지가 있으며, 아래에는 "error messaging the mach port for IMCRunLoopWakeUpReliable" 같은 실행 중 오류가 있고, 오른쪽에는 서로 다른 항목의 데이터 양과 속도 같은 처리 진행 상황과 데이터 전송 수치가 나열됩니다. 전반적으로 데이터셋 업로드 처리 중의 실행 상태 기록을 나타냅니다.](../../en/images/d28-01.png)

- 수동 업로드

데이터셋을 수집할 때 `push_to_hub=false`로 설정하면 수집이 끝난 뒤 수동으로 업로드합니다

```Shell
hf upload Tommymy/lerobot_my_dataset_a /Users/tommy/.cache/huggingface/lerobot/Tommymy/lerobot_my_dataset_a / --repo-type=dataset
```



자동이든 수동이든 업로드 속도는 매우 느립니다(초당 약 100 KB)

HuggingFace 서버가 해외에 있기 때문입니다

# 방법 2: 클라우드 GPU 플랫폼에서 업로드(권장)

## 클라우드 GPU 플랫폼 Featurize 로그인

https://featurize.cn?s=d7ce99f842414bfcaea5662a97581bd1

## 클라우드 GPU 인스턴스 시작

## 데이터셋 압축 파일을 `Datasets`에 업로드

## 인스턴스 다운로드 명령 복사

![이 이미지는 Featurize 플랫폼의 데이터셋 페이지를 보여 줍니다. 상단에는 "Datasets" 제목이 표시되고, 아래에는 "soarm_amazing_hand_pick.zip"이라는 데이터셋이 213.3 MB 크기로 16시간 전에 업로드되어 있습니다. 오른쪽에는 "Cloud Unzip" 버튼과 "Like", "Comment", "Copy Instance Download Command" 같은 버튼이 있습니다. 이 이미지는 "데이터셋 압축 파일을 `Datasets`에 업로드" 단계와 관련이 있으며, 업로드 후의 데이터셋 페이지를 보여 줍니다.](../../en/images/d28-02.png)

## 클라우드 GPU 인스턴스의 명령 줄에서 실행

```Shell
pip install httpx

unzip your_datasets.zip

hf auth login
```

## 데이터셋을 HuggingFace에 업로드

다음 내용으로 `upload_dataset.py` 파일을 만듭니다

```Python
from huggingface_hub import HfApi

api = HfApi()

api.upload_folder(
    folder_path="~/lerobot_my_dataset_a",
    repo_id="Tommymy/lerobot_my_dataset_a",
    repo_type="dataset"
)

api.create_tag("Tommymy/lerobot_my_dataset_a", tag="v0.4.0", repo_type="dataset")
```

파일을 실행합니다

```Shell
python upload_dataset.py
```

![이 이미지는 클라우드 GPU 인스턴스의 명령 줄에서 데이터셋 업로드를 실행하는 과정을 보여 주며, lerobot2라는 사용자가 python upload.py 명령을 실행했습니다. 파일 처리 진행 상황이 표시되는데, 처리할 파일이 6개이고 모두 100% 진행률이며, 각 파일의 전송 크기와 전체 데이터 전송 진행률 100%가 표시됩니다. 하단에는 마지막 커밋 이후 수정된 파일이 없어 빈 커밋이 생기지 않도록 커밋을 건너뛴다는 설명이 있습니다. 이 내용은 upload_dataset.py 파일을 실행하는 단계에 해당합니다.](../../en/images/d28-03.png)

- 또 다른 업로드 방법(권장하지 않음)

```Shell
hf upload Tommymy/lerobot_my_dataset_a lerobot_my_dataset_a / --repo-type=dataset
```

![이 이미지는 HuggingFace의 `hf upload` 명령으로 데이터셋을 업로드하는 과정을 보여 주며, 명령은 `hf upload TommyZihao/lerobot_zihao_dataset_a --repo-type=dataset`입니다. 이미지는 업로드가 마지막 단계에 들어갔음을 보여 주는데, 여러 비디오 파일과 parquet 파일을 포함한 모든 파일이 100% 진행률이며 업로드 크기가 해당 로컬 파일 크기와 정확히 일치하고, 총 업로드 파일 크기와 전송 속도, 그리고 하단에 이 업로드 커밋의 HuggingFace 데이터셋 페이지 링크가 표시되어 업로드 작업이 완료되었음을 나타냅니다.](../../en/images/d28-04.png)

# HuggingFace에서 데이터셋 보기

https://huggingface.co/datasets/Juxi-Technology/soarm_amazing_hand_pick

![이 이미지는 Hugging Face 플랫폼에서 Juxi-Technology 팀의 soarm_amazing_hand_pick 데이터셋 상세 페이지의 스크린샷으로, "HuggingFace에서 데이터셋 보기" 내용에 해당합니다. 상단에는 데이터셋의 내비게이션 옵션과 작성자, 태그 같은 기본 정보가 표시되고, 가운데 Dataset Viewer 영역에는 데이터셋의 split 1 훈련 데이터 일부가 action, observation_state, timestamp 같은 필드와 그 값과 함께 표시되며, 단일 레코드 크기, 총 레코드 수, 총 크기도 표시됩니다. 하단에는 이 데이터로 훈련된 관련 모델도 언급되어 있습니다.](../../en/images/d28-05.png)

![이 이미지는 Hugging Face 플랫폼의 soarm_amazing_hand_pick 데이터셋 페이지를 보여 줍니다. 상단에는 모델, 데이터셋 등을 검색하기 위한 검색 상자와 내비게이션 바가 있습니다. 데이터셋 정보 섹션에는 소유 조직 Juxi - Technology와 robotics, imitation-learning 같은 태그가 표시됩니다. "Files and versions" 탭 아래에는 data, meta, videos 같은 폴더와 README.md 파일이 나열되어 있고, 업로더, 업로드 방법, 시각이 "Upload README.md with huggingface_hub"처럼 표시됩니다. 이 이미지는 HuggingFace 데이터셋 보기와 관련이 있으며, 데이터셋의 파일과 버전을 시각적으로 보여 줍니다.](../../en/images/d28-06.png)
