[English](../en/06-teleoperation-with-camera/windows.md) | [简体中文](../zh-hans/06-teleoperation-with-camera/windows.md) | [繁體中文](../zh-hant/06-teleoperation-with-camera/windows.md) | [Deutsch](../de/06-teleoperation-with-camera/windows.md) | [Español](../es/06-teleoperation-with-camera/windows.md) | [Français](../fr/06-teleoperation-with-camera/windows.md) | [Italiano](../it/06-teleoperation-with-camera/windows.md) | [日本語](../ja/06-teleoperation-with-camera/windows.md) | 한국어 | [Português (BR)](../pt-br/06-teleoperation-with-camera/windows.md) | [Português (PT)](../pt-pt/06-teleoperation-with-camera/windows.md)

# Windows 컴퓨터

## 카메라를 컴퓨터에 연결

```Shell
lerobot-find-cameras opencv
```

![이 이미지는 Windows 시스템의 명령줄 오류 인터페이스로, 문서에서 언급한 "카메라가 연결되지 않음" 오류 시나리오에 대응합니다. 인터페이스에 표시된 오류 메시지는 두 부분으로, 하나는 전역 오류 "Camera index out of range", 다른 하나는 lerobot 관련 스크립트 오류 "Failed to connect or configure OpenCV camera 0"입니다. 인터페이스에는 또한 감지된 카메라 정보가 나열되어 있으며, OpenCV Camera라는 이름의 장치 2개가 모두 DSHOW 백엔드, YUY2 형식, 너비 640, 높이 480, 프레임 레이트 0.0FPS를 사용합니다. 이 오류 내용은 문서에서 설명한 카메라가 연결되지 않지만 Tencent Meeting에서는 정상적으로 사용되는 상황에 대응합니다.](../../en/images/d22-01.png)

## 카메라 화면을 표시하며 텔레오퍼레이션

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```

rerun.io 창이 열리고, 각 서보 관절의 궤적이 실시간으로 표시되며 카메라 화면도 함께 보입니다

그리고 이미지를 `C:\Users\username\outputs\captured_images` 디렉터리에 저장합니다

![이 이미지는 각 서보 관절의 궤적과 카메라 실시간 화면을 실시간으로 표시하는 rerun.io 화면을 보여 줍니다. 화면 왼쪽은 블루프린트 인터페이스로, "teleoperation" 등의 옵션이 있습니다. 가운데는 "observation_wrist_rot.pos" 등 관절 위치 데이터를 보여 주는 궤적 차트입니다. 오른쪽은 로봇 시점의 장면을 보여 주는 카메라 화면입니다. 화면 오른쪽 상단에는 "Waiting for data on rerun: http://127.0.0.1:9876/remote..."가 표시되고, 아래에는 데이터 소스 정보가 있습니다. 이 이미지는 rerun.io 화면이 카메라 화면을 실시간으로 표시하는 내용을 설명하는 부분과 관련이 있으며, 그 효과를 시각적으로 보여 줍니다.](../../en/images/d22-02.png)

## 다음과 같은 오류가 발생하면

카메라가 연결되지 않지만 Tencent Meeting에서 카메라를 바꾸면 여전히 정상적으로 열립니다

![이 이미지는 Windows 컴퓨터의 명령줄 인터페이스로, 카메라 감지 결과를 보여 줍니다. 인터페이스 위쪽에는 "관리자: 명령 프롬프트 - C:\\Users\\40743\\miniconda3\\condabin\\conda.bat activate leroobot"이라는 문구가 있습니다. 아래에는 "Detected Cameras" 정보가 출력되어 "Camera #0"이 OpenCV Camera @ 0, Type이 OpenCV, Backend api가 DSHOW, Default stream profile 등의 파라미터로 나열되며, "ERROR:lerobot.scripts.lerobot_find_cameras:Failed to connect or configure OpenCV camera 0: Failed to open OpenCVCamera(0)" 등의 오류 안내도 표시됩니다. 이 이미지는 문서에서 카메라가 연결되지 않는 문제의 맥락과 관련이 있으며, 관련 오류 정보를 시각적으로 보여 줍니다.](../../en/images/d22-03.png)

`lerobot\src\lerobot\cameras\utils.py` 파일을 수정하여 OpenCV 백엔드를 `cv2.CAP_SHOW`로 변경합니다

![이 이미지는 lerobot 라이브러리의 `utils.py` 파일을 수정하는 데 사용되는 Python 코드 일부를 보여 주며, 문서에서 Windows 컴퓨터의 카메라 연결 오류에 대한 해결 단계에 대응합니다. 코드는 `get_cv2_backend()` 함수를 정의하고, 시스템별로 OpenCV 백엔드를 설정합니다. 시스템이 Windows일 때 원래 주석 처리되었던 `return int(cv2.CAP_MSMF)`가 주석 처리되고, 현재 강조 표시된 내용은 `return int(cv2.CAP_DSHOW)`이며, 옆에 이 방식을 AVFOUNDATION 대신 사용한다는 설명이 붙어 있습니다. 이 함수는 또한 macOS와 Linux 시스템에 대응하는 OpenCV 백엔드도 설정하며, 해당 카메라 연결 문제를 해결하는 핵심 코드 수정 내용입니다.](../../en/images/d22-04.png)

> 이는 Doubao조차 해결할 수 없는 버그로, 모두 lerobot 라이브러리가 너무 깊게 감싸여 있어 초보자가 디버깅하기 매우 어렵기 때문입니다

<figure view-type="Preview">[Attachment: wx_camera_1768139334330.mp4](../../en/images/wx_camera_1768139334330.mp4)</figure>

## 카메라 여러 대를 연결, 카메라 화면을 표시하며 텔레오퍼레이션

```Shell
lerobot-teleoperate --robot.type=so101_follower --robot.port=COM6 --robot.id=my_follower_arm --robot.cameras="{ front: {type: opencv, index_or_path: 0, width: 640, height: 480, fps: 60, fourcc: "MJPG"}, side: {type: opencv, index_or_path: 1, width: 640, height: 480, fps: 60, fourcc: "MJPG"}}" --teleop.type=so101_leader --teleop.port=COM7 --teleop.id=my_leader_arm --display_data=true
```
