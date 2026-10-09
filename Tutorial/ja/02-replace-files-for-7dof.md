[English](../en/02-replace-files-for-7dof.md) | [简体中文](../zh-hans/02-replace-files-for-7dof.md) | [繁體中文](../zh-hant/02-replace-files-for-7dof.md) | [Deutsch](../de/02-replace-files-for-7dof.md) | [Español](../es/02-replace-files-for-7dof.md) | [Français](../fr/02-replace-files-for-7dof.md) | [Italiano](../it/02-replace-files-for-7dof.md) | 日本語 | [한국어](../ko/02-replace-files-for-7dof.md) | [Português (BR)](../pt-br/02-replace-files-for-7dof.md) | [Português (PT)](../pt-pt/02-replace-files-for-7dof.md)

# ステップ2：ファイルの置き換え（7DOF 対応）

## 1. 公式コードリポジトリから clone した場合に置き換え／変更すべきファイル

### 案 A：本リポジトリのコードを直接使う（推奨）

<figure view-type="Card">[Attachment: lerobot-7dof.zip](../en/images/lerobot-7dof.zip)</figure>

### 案 B：公式 lerobot を git clone して手動で置き換える

本リポジトリから公式 clone の**3 ファイル**を上書きします：

| 本リポジトリのファイル（コピー元） | 上書き先（コピー先） |
|-|-|
| `src/lerobot/robots/so_follower/so_follower.py` | 公式 clone の同名ファイル |
| `src/lerobot/teleoperators/so_leader/so_leader.py` | 公式 clone の同名ファイル |

<figure view-type="Card">[Attachment: so_follower.py](../en/images/so_follower.py)</figure>

<figure view-type="Card">[Attachment: so_leader.py](../en/images/so_leader.py)</figure>

> 前提：公式 clone が本リポジトリのベースライン（lerobot 2026-09 版）と同じ構造であること。
> 
> バージョン差が大きい場合は、**ファイル全体を上書きしないで**ください。代わりに以下の「手動変更」の 2 箇所を修正してください。

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
