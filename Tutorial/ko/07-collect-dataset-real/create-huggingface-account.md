[English](../en/07-collect-dataset-real/create-huggingface-account.md) | [简体中文](../zh-hans/07-collect-dataset-real/create-huggingface-account.md) | [繁體中文](../zh-hant/07-collect-dataset-real/create-huggingface-account.md) | [Deutsch](../de/07-collect-dataset-real/create-huggingface-account.md) | [Español](../es/07-collect-dataset-real/create-huggingface-account.md) | [Français](../fr/07-collect-dataset-real/create-huggingface-account.md) | [Italiano](../it/07-collect-dataset-real/create-huggingface-account.md) | [日本語](../ja/07-collect-dataset-real/create-huggingface-account.md) | 한국어 | [Português (BR)](../pt-br/07-collect-dataset-real/create-huggingface-account.md) | [Português (PT)](../pt-pt/07-collect-dataset-real/create-huggingface-account.md)

# Hugging Face 계정 등록(선택 사항)

## 중국 내 HuggingFace 미러 설정

- Ubuntu

```Shell
sudo nano ~/.bashrc

# 파일 끝에 다음을 추가합니다
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.bashrc
echo $HF_ENDPOINT

# 출력
# https://hf-mirror.com
```

- Mac

```Shell
sudo nano ~/.zshrc

# 파일 끝에 다음을 추가합니다
export HF_ENDPOINT=https://hf-mirror.com
```

```Shell
source ~/.zshrc

# 출력
# https://hf-mirror.com
```



## 토큰 생성

https://huggingface.co/settings/tokens

![이 이미지는 Hugging Face 웹사이트의 "토큰 설정" 페이지를 보여 줍니다. 페이지 왼쪽에는 사용자 아바타와 개인 정보가 있고, 오른쪽은 "사용자 액세스 토큰" 영역으로, 액세스 토큰이 인증에 사용되며 토큰 권한에 따라 애플리케이션이 작업을 수행하도록 허용한다고 설명하고, 토큰을 타인과 공유하지 말 것을 강조합니다. 페이지 오른쪽 상단에는 새 토큰을 생성하는 "+ Create new token" 버튼이 있습니다. 이 이미지는 문서의 "토큰 생성" 부분 내용과 관련이 있으며, 토큰 생성의 진입점을 시각적으로 보여 줍니다.](../../en/images/d24-01.png)

<grid>
<column width-ratio="0.606927">
![이 이미지는 Hugging Face 플랫폼에서 새 액세스 자격 증명(Token)을 생성하는 페이지를 보여 줍니다. 페이지의 "Token type" 영역에는 "Write" 옵션이 강조 표시되어 있고, "Token name" 입력란에는 "lerobot_zihao_token"이 입력되어 있으며, 페이지 아래쪽에는 생성을 제출하는 "Create token" 버튼이 있고, 페이지 왼쪽에는 현재 로그인한 사용자의 계정 정보가 표시됩니다.](../../en/images/d24-02.png)
</column>
<column width-ratio="0.393073">
![이 이미지는 Hugging Face 웹사이트의 "Save your Access Token" 팝업 화면을 보여 줍니다. 팝업은 사용자에게 토큰 값을 안전한 곳에 보관하라고 안내하며, 닫으면 다시 볼 수 없습니다. 아래에는 토큰 값 "<your-huggingface-token>"가 표시되고, 토큰을 복사하는 "Copy" 버튼이 있습니다. 아래에는 토큰 이름 "lerobot_zihao_token" 및 권한 "WRITE"도 있습니다. 이 이미지는 문서의 "토큰 생성" 단계와 관련이 있으며, 생성된 토큰을 기록하도록 사용자를 안내합니다.](../../en/images/d24-03.png)
</column>
</grid>

## 토큰 기록

예를 들어 제 것은 다음과 같습니다:

```Shell
<your-huggingface-token>
```

## 토큰 바인딩

```Shell
hf auth login

hf auth whoami
```

![이 이미지는 Mac 시스템에서 명령줄로 Hugging Face 계정을 조작하는 인터페이스를 보여 줍니다. 먼저 "hf auth login" 명령을 실행하면 이미 토큰이 있다는 안내가 나오고, "hf auth whoami"로 정보를 확인하거나 "hf auth logout"으로 로그아웃할 수 있습니다. 이어서 토큰을 입력하고 확인하면 토큰이 지정된 경로에 저장되었고 현재 활성 토큰이 "lerobot zihao token"이라고 표시되며, 마지막으로 "hf auth whoami" 명령을 실행하면 사용자가 "TommyZihao"로 표시됩니다. 이 이미지는 문서의 "토큰 생성" 부분에 대응하며, 토큰 생성 작업과 결과를 시각적으로 보여 줍니다.](../../en/images/d24-04.png)

## 데이터셋 저장소 생성

<grid>
<column width-ratio="0.480604">
![이것은 Hugging Face 플랫폼 인터페이스 스크린샷으로, 페이지 왼쪽 상단에는 Community, Docs, Pricing 등의 기능 진입점이 있고, 개인 계정 TommyZihao의 정보는 인터페이스 가운데 영역에 있으며, 초록색 박스로 표시된 것은 페이지 오른쪽 상단의 사용자 아바타 아이콘입니다. 인터페이스 가운데 메뉴에서 빨간색 박스로 "+ New Dataset" 옵션이 강조 표시되어 있으며, 이 옵션은 문맥의 "데이터셋 저장소 생성" 작업 단계에 대응하며, 이 옵션을 클릭하면 데이터셋 코드 저장소 생성을 시작할 수 있습니다.](../../en/images/d24-05.png)
</column>
<column width-ratio="0.519396">
![이 이미지는 Hugging Face에서 새 데이터셋 저장소를 생성하는 인터페이스를 보여 줍니다. 인터페이스의 "Dataset name"에 "lerobot_zihao_dataset_a"가 입력되어 있고, "License"는 "mit"로 선택되어 있으며, "Public" 옵션이 체크되어 있습니다. 하단에는 "Create dataset" 버튼이 있습니다. 이 이미지는 문서의 "데이터셋 저장소 생성" 부분 내용에 대응하며, 데이터셋 저장소를 생성할 때 입력해야 할 핵심 정보를 시각적으로 보여 주어 사용자가 Hugging Face에서 데이터셋 저장소를 생성하는 조작 인터페이스와 입력해야 할 필드를 이해하도록 돕습니다.](../../en/images/d24-06.png)
</column>
</grid>

![이것은 Hugging Face 플랫폼의 웹 인터페이스로, 사용자 TommyZhao가 생성한 lexobot_zihao_dataset_a라는 Dataset 저장소 내용을 보여 주며, Dark 테마 모드입니다. 페이지 왼쪽에는 "Edit Dataset Card" 편집 옵션이 있고, 데이터셋 파일을 푸시하는 여러 조작 방식(CLI, Python, HTTPS, SSH 관련 명령 예시)도 제공하며, 해당 데이터셋의 License가 mit로 표시되어 있습니다.](../../en/images/d24-07.png)
