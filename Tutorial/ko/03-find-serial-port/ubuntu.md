[English](../en/03-find-serial-port/ubuntu.md) | [简体中文](../zh-hans/03-find-serial-port/ubuntu.md) | [繁體中文](../zh-hant/03-find-serial-port/ubuntu.md) | [Deutsch](../de/03-find-serial-port/ubuntu.md) | [Español](../es/03-find-serial-port/ubuntu.md) | [Français](../fr/03-find-serial-port/ubuntu.md) | [Italiano](../it/03-find-serial-port/ubuntu.md) | [日本語](../ja/03-find-serial-port/ubuntu.md) | 한국어 | [Português (BR)](../pt-br/03-find-serial-port/ubuntu.md) | [Português (PT)](../pt-pt/03-find-serial-port/ubuntu.md)

<title>Ubuntu</title>

# 방법 1: Linux 명령 줄에서 직접 확인

## 시리얼 장치 포트 확인

```Shell
ls /dev/ttyACM*
```

## 컴퓨터와 로봇 암의 USB 포트 연결

Follower 암을 먼저 꽂고, 그다음 Leader 암을 꽂습니다

![이 이미지는 Ubuntu에서 Linux 명령 줄로 시리얼 장치 포트를 확인하는 과정을 보여 줍니다. 먼저 "아무것도 꽂지 않은" 상태를 보여 주고, Follower 암을 꽂은 뒤 "/dev/ttyACM0"이 나타나며, 그다음 Leader 암을 꽂은 뒤 "/dev/ttyACM1"이 나타납니다. 이 이미지는 맥락과 밀접하게 관련되어 있으며, 컴퓨터와 로봇 암의 USB 포트를 연결한 뒤 명령 줄에서 시리얼 장치 포트가 어떻게 보이는지 시각적으로 보여 주어 Linux 명령 줄에서 시리얼 장치 포트를 확인하는 방법을 설명하는 데 도움을 줍니다.](../../en/images/d08-01.png)

# 방법 2: 공식 LeRobot 도구

```Shell
lerobot-find-port
```

![이 이미지는 Ubuntu에서 공식 LeRobot 도구로 시리얼 장치 포트를 확인하는 명령 줄 인터페이스를 보여 줍니다. 명령은 "lerobot-find-port"이며, 여러 "/dev/ttyACM" 포트를 포함하여 사용 가능한 모든 포트를 표시합니다. 프롬프트는 Follower의 USB 케이블을 뽑고 해당 포트 번호 "/dev/ttyACM0"을 찾으라고 안내한 뒤 USB 케이블을 다시 꽂으라고 합니다. 이 이미지는 시리얼 장치 포트를 확인하는 두 번째 방법과 관련이 있으며, 단계와 결과를 시각적으로 보여 줍니다.](../../en/images/d08-02.png)

![이 이미지는 Ubuntu에서 `lerobot-find-port` 명령으로 로봇 암의 시리얼 장치 포트를 확인하는 과정을 보여 줍니다. 명령이 실행된 뒤 사용 가능한 모든 포트를 나열하고, Leader의 USB 케이블을 뽑으라고 안내하며, 마지막으로 Leader 암의 시리얼 장치 포트 번호로 `/dev/ttyACM1`을 보여 줍니다. 이 이미지는 시리얼 장치 포트를 확인하는 두 번째 방법과 관련이 있으며, 공식 LeRobot 도구로 포트 번호를 얻은 결과를 시각적으로 보여 줍니다.](../../en/images/d08-03.png)

# 내 포트 기록

`/dev/ttyACM0`은 Follower 암의 시리얼 장치 포트 번호입니다

`/dev/ttyACM1`은 Leader 암의 시리얼 장치 포트 번호입니다

# 포트에 권한 부여

모든 사용자에게 이 시리얼 장치의 읽기 및 쓰기 권한을 부여합니다

```Shell
sudo chmod 666 /dev/ttyACM*
```
