[English](../en/03-find-serial-port/mac.md) | [简体中文](../zh-hans/03-find-serial-port/mac.md) | [繁體中文](../zh-hant/03-find-serial-port/mac.md) | [Deutsch](../de/03-find-serial-port/mac.md) | [Español](../es/03-find-serial-port/mac.md) | [Français](../fr/03-find-serial-port/mac.md) | [Italiano](../it/03-find-serial-port/mac.md) | [日本語](../ja/03-find-serial-port/mac.md) | 한국어 | [Português (BR)](../pt-br/03-find-serial-port/mac.md) | [Português (PT)](../pt-pt/03-find-serial-port/mac.md)

# Mac 컴퓨터

## 포트 확인

```Shell
ls /dev/tty.*
```

결과는 아래 이미지와 비슷하게 나오며, 두 포트 중 어느 것을 사용해도 됩니다

![이 이미지는 Mac 터미널에서 "ls /dev/tty.*" 명령을 실행한 뒤의 출력을 보여 주며, 여러 포트가 나열되어 있습니다. 그중 "/dev/tty.usbmodem5AAF2194741"과 "/dev/tty.wchusbserial5AAF2194741" 두 개가 빨간 박스로 강조되어 있습니다. 관련 맥락은 포트 확인을 언급하며, 결과가 이 이미지와 비슷하고 두 포트 중 어느 것을 사용해도 됩니다. 이 이미지는 확인해야 할 포트 정보를 시각적으로 보여 주며, 위의 "포트 확인" 내용과 밀접하게 관련된 포트 확인 작업의 결과 화면입니다.](../../en/images/d09-01.png)

## 포트에 권한 부여

모든 사용자에게 이 시리얼 장치의 읽기 및 쓰기 권한을 부여합니다

```Shell
chmod 666 /dev/tty.*
```

## 내 포트 기록

Follower 암:

/dev/tty.usbmodem5AAF2193061

/dev/tty.wchusbserial5AAF2193061

Leader 암:

/dev/tty.usbmodem5AAF2194741

/dev/tty.wchusbserial5AAF2194741

## Mac에서는 왜 포트가 두 개일까요?

우리가 사용하는 서보 컨트롤러 보드는 **macOS에서 동시에 두 가지 종류의 시리얼 드라이버로 인식**되므로 포트가 두 개로 표시됩니다:

- 하나는 시스템의 기본 범용 시리얼 드라이버입니다(`/dev/tty.usbmodemxxxx`)
- 다른 하나는 칩 벤더가 제공하는 전용 시리얼 드라이버입니다(예를 들어 여기서 "wch"는 난징 친헝(Nanjing Qinheng)의 CH340/CH341 칩을 가리킵니다)(`/dev/tty.wchusbserialxxxx`)

이것은 정상이며, **두 포트는 실제로 같은 하드웨어 장치에 대응**하므로 둘 중 어느 것으로도 연결하고 통신할 수 있습니다(예를 들어 로봇 암을 제어하는 소프트웨어에서 두 포트 중 아무거나 선택하면 됩니다).

나중에 한 포트를 사용하다 오류가 발생하면 다른 포트로 바꿔 보십시오.
