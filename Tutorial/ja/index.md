[English](../en/index.md) | [简体中文](../zh-hans/index.md) | [繁體中文](../zh-hant/index.md) | [Deutsch](../de/index.md) | [Español](../es/index.md) | [Français](../fr/index.md) | [Italiano](../it/index.md) | 日本語 | [한국어](../ko/index.md) | [Português (BR)](../pt-br/index.md) | [Português (PT)](../pt-pt/index.md)

<title>SO-ARM101 ロボットアーム 7 軸チュートリアル</title>

# SO-ARM101 7-DOF · 実行前の準備

> 本説明は、**SO-ARM101 を 6 サーボから 7 サーボに改造**した後、LeRobot で一連の流れ（キャリブレーション → 記録 → 学習 → デプロイ）を実行する人向けのものです。  
> 対応するコード：本リポジトリ（`lerobot-7dof`）。これは公式 lerobot の fork で、SO 関連のモーター設定のみを変更しています。

---

## 0. まず自分のロボットアームを確認する

7 サーボ（すべて STS3215）。サーボ ID と関節の対応関係：

| **サーボ ID** | **関節名** | **説明** |
|-|-|-|
| 1 | `shoulder_pan` | 肩の水平回転 |
| 2 | `shoulder_lift` | 肩のリフト |
| 3 | `elbow_flex` | 肘の屈曲 |
| 4 | `wrist_flex` | 手首のピッチ（上下の曲げ） |
| 5 | `wrist_yaw` | 手首のヨー（左右に約 90° 回転）· **今回追加されたサーボ**（元の 4・5 番の間に挿入） |
| 6 | `wrist_roll` | 手首のロール · 元の 5 番ロールモーター。ID 5→6、印刷部品は変更なし、名前も変更なし |
| 7 | `gripper` | グリッパ · 元は ID=6、変更後は 7 に |

関節データの順序（記録後に Parquet 内の `action` / `observation.state` が持つ関節次元の順序）：  
`shoulder_pan → shoulder_lift → elbow_flex → wrist_flex → wrist_yaw → wrist_roll → gripper`。

⚠️ 注意：**6 サーボ版のデータ、キャリブレーションファイル、学習済みモデルは本リポジトリと互換性がありません**。以下の手順に従って必ずやり直してください。

---

## 1. 公式コードリポジトリから clone した場合に置き換え／変更すべきファイル

### 案 A：本リポジトリのコードを直接使う（推奨）

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### 案 B：公式 lerobot を git clone して手動で置き換える

本リポジトリから公式 clone の**3 ファイル**を上書きします：

| 本リポジトリのファイル（コピー元） | 上書き先（コピー先） |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | 公式 clone の同名ファイル |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | 公式 clone の同名ファイル |
| `src/lerobot/robots/so_follower/robot_kinematic_processor.py` | 公式 clone の同名ファイル（**コメント修正のみ**、機能に影響なし、置き換えなくても可） |

```Bash
cp src/lerobot/robots/so_follower/so_follower.py          <公式clone>/src/lerobot/robots/so_follower/so_follower.py
cp src/lerobot/teleoperators/so_leader/so_leader.py       <公式clone>/src/lerobot/teleoperators/so_leader/so_leader.py
```

> 前提：公式 clone が本リポジトリのベースライン（lerobot 2026-08 版）と同じ構造であること。バージョン差が大きい場合は、**ファイル全体を上書きしないで**ください。代わりに以下の「手動変更」の 2 箇所を修正してください。

### バージョンが一致しない場合の手動変更（2 箇所のみ）

**① モーターディクショナリ**（`so_follower.py` と `so_leader.py` に 1 つずつ、内容は同じ）——元の

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_roll": Motor(5, "sts3215", norm_mode_body),
"gripper": Motor(6, "sts3215", MotorNormMode.RANGE_0_100),
```

を

```Python
"wrist_flex": Motor(4, "sts3215", norm_mode_body),
"wrist_yaw": Motor(5, "sts3215", norm_mode_body),   # 追加サーボ、左右回転
"wrist_roll": Motor(6, "sts3215", norm_mode_body),  # 元の 5 番ロールモーター、ID 5→6
"gripper": Motor(7, "sts3215", MotorNormMode.RANGE_0_100),
```

に変更します。

**② キャリブレーションロジック**（両ファイルそれぞれの `calibrate()`）——「全周関節」の特判を削除します。以下を

```Python
full_turn_motor = "wrist_roll"
unknown_range_motors = [motor for motor in self.bus.motors if motor != full_turn_motor]
range_mins, range_maxes = self.bus.record_ranges_of_motion(unknown_range_motors)
range_mins[full_turn_motor] = 0
range_maxes[full_turn_motor] = 4095
```

1 行に置き換え、全関節の実際の範囲を記録するようにします：

```Python
range_mins, range_maxes = self.bus.record_ranges_of_motion()
```

> 理由：オリジナル版は `wrist_roll`（前腕軸まわりのロール）を全周（0\~4095）回転できる関節とみなし、全範囲をハードコードしていました。7-DOF 化後は 5・6 番の手首関節（yaw / roll）が**どちらも機械的リミットに当たり、全周回転できません**。全周をハード設定すると、コードが関節指令を機械的に到達できない角度へ送ってしまい、破損のリスクがあります。現在はキャリブレーション時に各モーターの実際の min/max を手動で記録します。

### デュアルフォロワーアーム の説明

`bi_so_follower` / `bi_so_leader`（`src/lerobot/robots/bi_so_follower/`、`src/lerobot/teleoperators/bi_so_leader/`）は、単腕を 1 層ラップして `left_`/`right_` プレフィックスを付けるだけで、**モーター定義は含みません**。**上記の単腕ファイル**を修正すれば、双腕コマンド（`--robot.type=bi_so_follower`）は自動的に 7-DOF になります。
