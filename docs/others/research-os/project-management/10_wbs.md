# 🧱 WBS YAML Schema Design
## AI管理用に最適化した `10_wbs.yaml` 設計

---

# 🎯 目的

`10_wbs.yaml` は、プロジェクトの **作業分解構造（WBS）** を管理する中核ファイルです。  
このファイルでは、単に「やることの一覧」を持つのではなく、

- 🤖 AIがタスクの意味を理解しやすい
- 👤 人間がレビューしやすい
- 📊 Schedule / Mermaid / Status Report に流用しやすい
- 🔍 Git diff で変更意図を追いやすい

という性質を重視します。

---

# 🧠 設計思想

WBS は単なるチェックリストではありません。  
ここで持つべきなのは、**構造としての真実**です。

つまり `10_wbs.yaml` では、

- **何をやるのか**
- **なぜそれが必要か**
- **どうなれば完了か**
- **何に依存しているか**

を定義します。

逆に、以下は原則として持ちすぎない方がよいです。

- 実績日
- 日々の作業ログ
- レビューコメント全文
- 詳細な会話履歴

これらは他ファイルへ分離します。

---

# 🏗️ スキーマ全体像

```yaml
project:
  id: string
  name: string
  description: string
  version: string

wbs:
  - id: string
    name: string
    type: epic | feature | task | review
    summary: string
    phase: string
    priority: high | medium | low
    estimate_days: number
    owner: string | null
    status: todo | doing | blocked | review | done
    depends_on:
      - string
    definition_of_done:
      - string
    deliverables:
      - string
    risks:
      - string
    notes:
      - string
    links:
      - string
    children:
      - ...
```

---

# 📂 ルート構造

## `project`

プロジェクト全体のメタ情報です。

```yaml
project:
  id: iTenFs
  name: iTenFs
  description: 編集データ永続化・表示・エクスポート対応
  version: 1
```

### 各項目の意味

- `id`
  - プロジェクト識別子
  - 短く固定的にする
- `name`
  - 人間が読む正式名称
- `description`
  - このWBSが何を対象にしているか
- `version`
  - スキーマや内容の更新管理用

---

# 🧱 タスク要素の基本スキーマ

各タスクは以下の構造を持ちます。

```yaml
- id: PR4-C1
  name: サムネイル生成ロジックの実装
  type: task
  summary: 編集済み画像を表示用サムネイルへ変換するロジックを実装する
  phase: PR4
  priority: high
  estimate_days: 2.0
  owner: yusuke
  status: todo
  depends_on:
    - PR3
  definition_of_done:
    - 編集済み画像からサムネイル生成できる
    - 一覧表示画面で利用できる
    - 既存データでクラッシュしない
    - 必要なテストが追加されている
  deliverables:
    - サムネイル生成ロジック
    - テストコード
  risks:
    - 大量画像時にパフォーマンス低下の可能性
  notes:
    - キャッシュ方式との整合が必要
  links:
    - docs/design/thumbnail.md
```

---

# 🔑 必須項目

AI運用の観点では、まず以下を必須にするのがおすすめです。

## `id`
一意の識別子です。

例:
- `PR2`
- `PR2-C1`
- `PR3-C4`

### ルール
- 安定した命名にする
- 後から変えない
- 人間にも意味がわかる粒度にする

---

## `name`
人間向けのタスク名です。

例:
```yaml
name: サムネイル生成ロジックの実装
```

### ポイント
- 実装対象がわかる
- 抽象すぎない
- 長すぎない

---

## `type`
タスク種別です。

```yaml
type: epic | feature | task | review
```

### 用途
- `epic`: PRや大機能単位
- `feature`: 機能まとまり
- `task`: 実装・調査・テストなどの具体作業
- `review`: レビュー対応

### なぜ必要か
AIが「親タスク」と「実作業タスク」を区別しやすくなります。

---

## `summary`
1〜2文で要旨を書く項目です。

```yaml
summary: 編集済み画像を表示用サムネイルへ変換するロジックを実装する
```

### なぜ必要か
`name` だけだと短すぎて、AIが誤解しやすいからです。  
`summary` があるとタスクの意味が安定します。

---

## `phase`
どのフェーズ・PRに属するかです。

```yaml
phase: PR4
```

これはタスク集計やガント生成時に便利です。

---

## `estimate_days`
見積です。

```yaml
estimate_days: 2.0
```

### ポイント
- 日単位で持つ
- 小数可
- 厳密すぎず比較可能にする

---

## `status`
状態です。

```yaml
status: todo
```

使用値は固定します。

```yaml
todo
doing
blocked
review
done
```

### なぜ固定するか
AIが
- `in progress`
- `working`
- `started`

のように揺らすのを防ぐためです。

---

# 🧩 強く推奨する項目

必須ではないが、AI運用の精度をかなり上げる項目です。

## `depends_on`
依存関係です。

```yaml
depends_on:
  - PR3-C5
```

### 効果
- ガント生成に使える
- 着手順序をAIが判断しやすい
- blocked理由の説明に使える

---

## `definition_of_done`
完了条件です。

```yaml
definition_of_done:
  - 編集済み画像からサムネイル生成できる
  - 一覧表示画面で利用できる
  - クラッシュしない
  - テスト追加済み
```

### これは非常に重要
AIは「コードを書いた」だけで完了扱いしがちです。  
そのため、**完了の意味を明文化**する必要があります。

---

## `deliverables`
成果物です。

```yaml
deliverables:
  - サムネイル生成ロジック
  - テストコード
```

### 効果
- 何が残るかがわかる
- 進捗報告しやすい
- レビュー対象が明確になる

---

## `risks`
そのタスク固有のリスクです。

```yaml
risks:
  - 大量画像時にパフォーマンス低下の可能性
```

### 効果
- リスク管理ファイルに流用できる
- AIが blocker 候補を整理しやすい

---

## `notes`
補足です。

```yaml
notes:
  - キャッシュ方式との整合が必要
```

### 位置づけ
自由記述ですが、長文は避けます。  
1〜3行程度の短い運用メモがちょうどよいです。

---

## `links`
関連リンクです。

```yaml
links:
  - docs/design/thumbnail.md
  - pulls/123
```

### 用途
- 設計書
- 関連PR
- 補足資料
- テスト方針

---

# 🌳 階層構造

WBSでは親子構造を持てるようにします。

例:

```yaml
wbs:
  - id: PR4
    name: 表示対応（サムネイル・一覧表示）
    type: epic
    summary: 編集済み画像を一覧画面とサムネイルへ反映する
    phase: PR4
    priority: high
    estimate_days: 8.5
    owner: yusuke
    status: doing
    depends_on:
      - PR3
    definition_of_done:
      - 一覧表示で編集済み画像が適切に見える
      - サムネイル生成が安定している
      - 表示関連のテストが追加されている
    deliverables:
      - 表示対応一式
    risks:
      - 画像数増加時の負荷
    notes: []
    links: []
    children:
      - id: PR4-C1
        name: サムネイル生成ロジックの実装
        type: task
        summary: 編集済み画像を表示用サムネイルへ変換するロジックを実装する
        phase: PR4
        priority: high
        estimate_days: 2.0
        owner: yusuke
        status: doing
        depends_on:
          - PR3
        definition_of_done:
          - 編集済み画像からサムネイル生成できる
          - 一覧表示画面で利用できる
          - クラッシュしない
          - 必要なテストが追加されている
        deliverables:
          - サムネイル生成ロジック
          - テストコード
        risks:
          - 大量画像時にパフォーマンス低下の可能性
        notes:
          - キャッシュ方式との整合が必要
        links:
          - docs/design/thumbnail.md
      - id: PR4-C2
        name: 一覧表示での編集済み画像表示
        type: task
        summary: 一覧画面で編集済み画像の見た目が反映されるようにする
        phase: PR4
        priority: high
        estimate_days: 1.5
        owner: yusuke
        status: todo
        depends_on:
          - PR4-C1
        definition_of_done:
          - 一覧で編集済み画像が反映される
          - 既存表示を壊していない
        deliverables:
          - 一覧表示対応
        risks: []
        notes: []
        links: []
```

---

# 🚦 priority の扱い

優先度も固定値にするのがおすすめです。

```yaml
priority: high | medium | low
```

理由:
- AIが順序付けしやすい
- ステータスレポートで拾いやすい
- 高優先タスクの遅延を検知しやすい

---

# 👤 owner の扱い

担当者を入れるかはチームサイズ次第ですが、将来的には有効です。

```yaml
owner: yusuke
```

単独開発なら省略も可能ですが、  
複数人を想定するなら早めに入れてよいです。

---

# ❌ 入れすぎない方がよいもの

WBSに全部入れたくなりますが、次は分離推奨です。

## 実績日
→ `20_schedule.yaml`

## 日々の作業ログ
→ `30_progress.yaml`

## 今週の要約
→ `50_status_report.md`

## 全体横断のリスク一覧
→ `60_risks_issues.md`

つまり、`10_wbs.yaml` は  
**構造の定義に集中**させます。

---

# ✅ 初期段階のおすすめ必須セット

初期運用で最低限必要なのはこれです。

```yaml
- id
- name
- type
- summary
- phase
- estimate_days
- status
- depends_on
- definition_of_done
- children
```

このセットがあるだけで、かなりAI管理しやすくなります。

---

# 🪜 段階的成熟モデル

## Phase 1: 最小運用
```yaml
id
name
type
summary
phase
estimate_days
status
children
```

## Phase 2: AI管理強化
```yaml
depends_on
definition_of_done
priority
```

## Phase 3: チーム運用強化
```yaml
owner
deliverables
risks
links
```

最初から全部埋めなくても大丈夫です。  
ただし `definition_of_done` は早めに入れる価値が高いです。

---

# 📌 おすすめ命名規則

```yaml
PR2
PR2-C1
PR2-C2
PR3
PR3-C1
PR4
PR4-C1
PR5-REVIEW
```

### ルール
- 親: `PRx`
- 子: `PRx-Cn`
- レビュー対応: `PRx-REVIEW`

この形だと、人間もAIも追いやすいです。

---

# 🎯 このスキーマの本質

この WBS スキーマの核心は、

> **AIが編集しやすいようにすることではなく、AIが誤解しにくいようにすること**

です。

そのために必要なのが、

- 名前だけでなく `summary`
- 完了したかを判定する `definition_of_done`
- 順序を支える `depends_on`
- 親子関係を示す `children`

です。

---

# ✅ 結論

`10_wbs.yaml` は、次の役割に集中させるのが最適です。

- 🧱 タスク構造を定義する
- 🎯 完了条件を定義する
- 🔗 依存関係を定義する
- 🤖 AIが誤解しにくい文脈を与える

一言でまとめると、

> **WBS は「やること一覧」ではなく、「AIと人間が共有する作業意味モデル」である**

---

# 🚀 推奨次アクション

次に作るべきものはこのどちらかです。

1. **あなたの PR2〜PR5 をこのスキーマで実際の `10_wbs.yaml` に落とした初期ドラフト**
2. **`20_schedule.yaml` のスキーマ設計**

おすすめ順は **1 → 2** です。  
まず WBS の中身を固めると、その後の Schedule と Mermaid が安定します。