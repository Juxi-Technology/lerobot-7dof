[English](../en/index.md) | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | [日本語](../ja/index.md) | 한국어 | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>SO-ARM101 7축 튜토리얼</title>

# SO-ARM101 7-DOF · 실행 전 준비

> 이 설명은 **SO-ARM101을 6개 서보에서 7개 서보로 개조한** 뒤 LeRobot으로 전체 워크플로(캘리브레이션 → 기록 → 학습 → 배포)를 실행하려는 분을 대상으로 합니다.  
> 대응 코드: 이 저장소(`lerobot-7dof`)로, 공식 lerobot의 fork이며 SO 관련 모터 설정만 변경했습니다.

---

## 0. 먼저 자신의 로봇 팔을 확인하세요

7개 서보(모두 STS3215), 서보 ID와 관절의 대응 관계:

| **서보 ID** | **관절 이름** | **설명** |
|-|-|-|
| 1 | `shoulder_pan` | 어깨 수평 회전 |
| 2 | `shoulder_lift` | 어깨 들어올림 |
| 3 | `elbow_flex` | 팔꿈치 굽힘 |
| 4 | `wrist_flex` | 손목 피치(위아래 굽힘) |
| 5 | `wrist_yaw` | 손목 요(좌우 약 90° 회전) · **이번에 추가된 서보**(기존 4·5번 사이에 삽입) |
| 6 | `wrist_roll` | 손목 롤 · 기존 5번 롤 모터, ID 5→6, 출력 부품은 변경 없음, 이름도 그대로 |
| 7 | `gripper` | 그리퍼 · 기존 ID=6, 변경 후 7로 순연 |

관절 데이터 순서(기록 후 Parquet의 `action` / `observation.state`에 담기는 관절 차원 순서):  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`.

⚠️ 주의: **6서보 버전의 데이터, 캘리브레이션 파일, 이미 학습된 모델은 이 저장소와 모두 호환되지 않습니다.** 아래 절차에 따라 처음부터 다시 해야 합니다.

---

## 1. 공식 코드 저장소에서 클론했다면, 어떤 파일을 교체/수정해야 하나

### 방안 A: 이 저장소의 코드를 그대로 사용(권장)

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### 방안 B: 공식 lerobot을 git clone한 뒤 수동으로 교체

이 저장소에서 공식 clone의 **파일 3개**를 덮어씁니다:

| 이 저장소 파일(원본) | 덮어쓸 대상(대상) |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | 공식 clone의 동일 경로 파일 |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | 공식 clone의 동일 경로 파일 |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | 공식 clone의 동일 경로 파일(**주석 수정만**, 기능에는 영향 없음, 교체하지 않아도 됨) |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <공식 클론>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <공식 클론>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> 전제: 공식 clone이 이 저장소의 기준(lerobot 2026-08 버전)과 구조가 같아야 합니다. 버전 차이가 크면 **파일 전체를 덮어쓰지 말고**, 아래 "수동 수정"의 두 곳만 고치면 됩니다.

### 버전이 맞지 않을 때의 수동 수정(두 곳만)

**① 모터 딕셔너리**(`so_follower.py`와 `so_leader.py`에 각각 하나씩, 내용은 동일) — 기존의

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

을(를) 다음과 같이 바꿉니다

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # 추가된 서보, 좌우 회전
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # 기존 5번 롤 모터, ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

**② 캘리브레이션 로직**(두 파일 각각의 `calibrate()`) — "한 바퀴 관절" 특례를 제거합니다: 다음을

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

다음 한 줄로 교체하여, 모든 관절의 실제 범위를 기록하도록 합니다:

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> 이유: 기존 버전은 `wrist_roll`(전완 축을 중심으로 회전)을 한 바퀴(0\~4095) 돌 수 있는 관절로 간주해 전체 범위를 하드코딩했습니다. 7-DOF로 개조한 뒤에는 5·6번 손목 관절(yaw / roll)이 **모두 기계적 리밋에 걸려 한 바퀴 회전이 불가능**합니다. 전체 범위를 강제로 설정하면 코드가 기계적으로 도달할 수 없는 각도로 관절 명령을 보내 손상 위험이 있습니다. 이제 캘리브레이션할 때 각 모터의 실제 min/max를 수동으로 기록합니다.

### 듀얼 Follower 암 설명

`bi_so_follower` / `bi_so_leader`(`src/lerobot/robots/bi_so_follower/`, `src/lerobot/teleoperators/bi_so_leader/`)는 단일 암을 한 겹 감싸 `left_`/`right_` 접두사를 붙인 것일 뿐, **모터 정의를 포함하지 않습니다**. **위의 단일 암 파일**만 제대로 고치면, 양팔 명령(`--robot.type=bi_so_follower`)은 자동으로 7-DOF가 됩니다.
