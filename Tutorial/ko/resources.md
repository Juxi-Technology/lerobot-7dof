[English](../en/resources.md) | [简体中文](../zh-hans/resources.md) | [繁體中文](../zh-hant/resources.md) | [Deutsch](../de/resources.md) | [Español](../es/resources.md) | [Français](../fr/resources.md) | [Italiano](../it/resources.md) | [日本語](../ja/resources.md) | 한국어 | [Português (BR)](../pt-br/resources.md) | [Português (PT)](../pt-pt/resources.md)

<title>몇 가지 리소스</title>

# Lerbot 공식 [URDF 파일](https://github.com/TheRobotStudio/SO-ARM100/blob/main/Simulation/SO101/so101_new_calib.urdf)



## URDF Studio

https://urdf.d-robotics.cc/



## ROS2 시뮬레이션 제어(직접 구현 가능)

https://github.com/holmsslk/so-arm-moveit-hardware



## LeRobot 공식 그래픽 인터페이스

https://github.com/huggingface/leLab

LeLab은 캘리브레이션, 원격조작, 기록, 학습, 재생 등 LeRobot의 모든 워크플로를 하나의 브라우저 인터페이스에 담은 웹 앱입니다. 로봇 팔을 연결하고 앱을 열기만 하면 바로 작업을 시작할 수 있습니다. 번거로운 명령줄 작업도, 키보드 입력도 필요하지 않습니다.

🤗 LeRobot의 네이티브 웹 진입점으로, 신규 사용자가 "개봉 직후" 상태에서 몇 분 만에 "첫 정책 학습"까지 도달하도록 설계되었습니다.

🤗 단 하나의 명령으로 모든 것을 설치하고 실행합니다.



# 스마트폰으로 Follower 암 제어하기

https://huggingface.co/docs/lerobot/main/en/phone_teleop





## 클라우드 로보틱스 개발: AWS에서 ROS 2 디바이스와 Isaac Sim의 LeRobot 시뮬레이션 및 데이터 스트리밍

https://github.com/ti/ti.github.io/blob/95261efa4f8bdb4c8571920762318a532e58c7fd/%E5%BC%80%E5%8F%91/isaac/aws-ros2-isaac.md



## 웹 UI에서 서보 ID 설정 및 중심 캘리브레이션

https://bambot.org/feetech.js?lang=zh

1、서보 모델에 따라 0 또는 1을 입력한 뒤 "연결"을 클릭합니다

![이 이미지는 웹 UI에서 서보 ID 설정 및 중심 캘리브레이션을 할 때의 연결 인터페이스를 보여 줍니다. 인터페이스에는 "연결" 부분이 있으며, 보드레이트 선택 상자(1,000,000 bps, Index 0), 프로토콜 엔드 선택 상자(0=STS/SMS, 1=SCS, 현재 0 선택), 연결 버튼(녹색, 숫자 2 표시), 상태 표시(빨간색, "연결 끊김" 표시)가 포함되어 있습니다. 이 이미지는 위에서 언급한 "서보 모델에 따라 0 또는 1을 입력하고 '연결'을 클릭"과 호응하며, 연결 조작의 인터페이스 설정 상황을 시각적으로 보여 줍니다.](../en/images/d58-01.png)

2、ID 1\~6의 서보를 스캔합니다. 스캔 결과의 FOUND를 보고 해당 ID 서보를 확인할 수 있습니다. 예를 들어 이미지에서는 서보 ID 1이 스캔되었습니다

![이것은 Lerbot 관련 소프트웨어의 서보 스캔 인터페이스 스크린샷으로, 핵심 내용은 서보 스캔의 조작 과정과 결과입니다. 인터페이스에는 "시작 ID" "종료 ID" 입력란이 있어 각각 1과 6이 입력되어 있고, "스캔 시작" 버튼과 그 아래에 대응하는 스캔 결과 표시 영역이 있으며, 이 위치는 문서에서 언급한 서보 ID가 스캔되는 관련 내용에 대응합니다. 스캔 결과 로그를 보면 ID1을 스캔할 때만 서보 정보를 성공적으로 얻어 "FOUND: Pos-1230, Mode=0, BaudIdx=0"으로 표시되고, ID2부터 ID6까지 스캔할 때는 모두 위치 읽기 예외, 상태 패킷 없음 오류 안내가 나타납니다.](../en/images/d58-02.png)

3、ID 설정 및 중심 캘리브레이션

①현재 서보 ID 입력란을 스캔된 서보의 ID로 입력합니다

②"ID 관리"에 숫자를 입력하고 "ID 변경"을 클릭하면 ID를 설정할 수 있습니다

③중심 캘리브레이션(STS3215 서보의 중심은 2047, SCS0009 서보의 중심은 511입니다)

STS 서보: "위치 제어"에 2047을 입력하고 "Set"을 클릭합니다

SCS 서보: "위치 제어"에 511을 입력하고 "Set"을 클릭합니다

![이 이미지는 Lerbot 공식 URDF Studio의 단일 서보 제어 인터페이스를 보여 줍니다. 인터페이스에서 "현재 서보 ID"가 1로 표시되고, 아래 "ID 관리"에서 숫자를 입력해 "ID 변경"을 클릭할 수 있습니다. "위치 제어" 영역에는 "위치 읽기" 버튼이 있어 위치가 2047로 표시되고, 옆에 "Set" 버튼이 있습니다. 이 이미지는 문맥과 긴밀하게 관련되어 있으며, 문맥에서 언급한 현재 서보 ID를 스캔된 서보 ID로 입력하기, "ID 관리"에 숫자를 입력하고 "ID 변경"을 클릭해 ID 설정하기, STS 서보는 "위치 제어"에 2047을 입력하고 "Set"을 클릭하기 등의 작업 단계를 시각적으로 보여 줍니다.](../en/images/d58-03.png)
