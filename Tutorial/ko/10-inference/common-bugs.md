[English](../en/10-inference/common-bugs.md) | [简体中文](../zh-hans/10-inference/common-bugs.md) | [繁體中文](../zh-hant/10-inference/common-bugs.md) | [Deutsch](../de/10-inference/common-bugs.md) | [Español](../es/10-inference/common-bugs.md) | [Français](../fr/10-inference/common-bugs.md) | [Italiano](../it/10-inference/common-bugs.md) | [日本語](../ja/10-inference/common-bugs.md) | 한국어 | [Português (BR)](../pt-br/10-inference/common-bugs.md) | [Português (PT)](../pt-pt/10-inference/common-bugs.md)

# 흔한 버그와 해결 방법

## 카메라 캡처 실패

![이 이미지는 LeRobot 로봇 코드가 실행될 때의 터미널 출력을 보여줍니다. `INFO` 로그는 OpenCV 카메라가 열리고 Follower가 연결 해제되는 것을 보여주며, `ERROR` 로그는 `camera_opencv.py` 파일에서 `read` 함수가 `OpenCVCamera(0) read failed`로 인해 `RuntimeError`를 발생시켰다고 지적합니다. 이 이미지는 "카메라 캡처 실패" 문제와 관련되어, 코드 실행 시 나타나는 오류 상황을 시각적으로 보여주고 이 문제가 실제 실행에서 어떻게 나타나는지 이해하도록 돕습니다.](../../en/images/d55-01.png)

손목 카메라의 배선이 느슨해졌는지 확인하세요. 특히 카메라에 가까운 쪽 끝의 배선이 접촉 불량이 매우 잘 생깁니다

## 카메라 연결 끊김

![이 이미지는 LeRobot HF를 실행하는 과정에서 나타난 카메라 캡처 실패 문제의 코드 및 오류 정보 인터페이스를 보여 줍니다. 인터페이스 위쪽에는 OpenCV 카메라 연결 실패 안내가 표시되고, 아래쪽은 `lerobot_record.py` 파일의 `record_loop` 함수 오류 스택으로, `async_read` 시 `TimeoutError`가 발생하여 카메라가 200ms 이내에 프레임을 가져오지 못했음을 알려 줍니다. 이 이미지는 문서의 "카메라 캡처 실패" 문제 맥락에 대응하며, 이 문제가 코드 수준에서 구체적으로 어떻게 나타나는지 시각적으로 보여 줍니다.](../../en/images/d55-02.png)

명령줄을 다시 시작하세요

## 서보 통신 문제 1

ConnectionError: Failed to sync read 'Present_Position' on ids=[1, 2, 3, 4, 5, 6] after 1 tries. [TxRxResult] There is no status packet!

![이 이미지는 터미널 오류 로그의 일부로, 문서에서 언급한 서보 통신 문제 1 관련 내용에 대응합니다. 로그에는 구체적인 코드 파일 경로가 표시되어 있으며, `lerobot/src/lerobot/motors/motors_bus.py` 파일의 1069번째 줄, 1181번째 줄에 `ConnectionError: Failed to sync read 'Present_Position' on ids=\[1,2,3,4,5,6\] after 1 tries` 오류 정보가 기록되어 있고, `There is no status packet!` 오류 원인도 표시되어 문서에서 언급한 서보 통신 문제 1의 오류 상세를 온전히 보여 줍니다.](../../en/images/fix-04.png)

해결 방법: `lerobot/src/lerobot/motors/motors_bus.py` 코드에서 모든 `num_retry`를 99로 변경합니다. 특히 오류가 발생한 줄에 해당하는 값을 그렇게 바꾸세요

![이 이미지는 LeRobot 프로젝트의 `motors_bus.py` 코드 파일 내용을 보여 줍니다. 파일에서 `MotorsBusABC` 클래스의 `write` 메서드가 강조 표시되어 있고, 그중 `num_retry` 변수가 `99`로 수정되어 있습니다. 이 이미지는 문서의 "서보 통신 문제 1" 부분과 관련이 있으며, 해결 방법에서 언급한 `lerobot/src/lerobot/motors/motors_bus.py` 코드의 모든 `num_retry`를 99로 바꾸는 작업, 특히 오류가 발생한 줄에 해당하는 코드 부분에 대응합니다.](../../en/images/d55-03.png)

## 서보 통신 문제 2

ConnectionError: Failed to write 'Torque_Enable' on id\_=1 with '0' after 6 tries. [TxRxResult] There is no status packet!

<grid>
<column width-ratio="0.500000">
![이 이미지는 오류 로그의 일부로, 문서의 "서보 통신 문제 2"에 대응하는 오류 내용입니다. 구체적으로 코드 실행 시 ConnectionError라는 오류가 발생했으며, 증상은 Failed to write 'Torque_Enable' on id=1 with '0' after 6 tries이고, 함께 표시된 오류 정보는 There is no status packet을 설명하며, 해당 오류를 발생시킨 코드의 경로도 표시되어 있습니다. 이 오류 로그는 문서의 "서보 통신 문제 2" 설명과 완전히 일치하며, 이 문제의 실제 오류 기록입니다.](../../en/images/d55-04.png)
</column>
<column width-ratio="0.500000">
![이 이미지는 macOS 시스템에서 zsh 터미널로 명령줄 작업을 수행하는 인터페이스를 보여 줍니다. 터미널에는 여러 파일 경로와 코드 파일 내용이 표시되어 있으며, 그중 `/Users/tommy/Downloads/9-lerobot/lerobot/src/lerobot/motors/motors_bus.py` 파일에 `ConnectionError: Failed to write 'Torque_Enable' on id_=1 with '0' after 6 tries. \[TxRxResult\] There is no status packet!` 오류 정보가 있습니다. 이 이미지는 문서의 "서보 통신 문제 2" 내용과 관련이 있으며, 오류 발생 시의 터미널 인터페이스 상황을 시각적으로 보여 줍니다.](../../en/images/fix-05.png)
</column>
</grid>

해결 방법: 로봇 팔을 다시 캘리브레이션합니다
