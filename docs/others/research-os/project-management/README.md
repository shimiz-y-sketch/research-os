# 📂 Project Management

このディレクトリは、プロジェクト管理情報を  
**YAML + Mermaid + Markdown** で管理するための場所です。

Excel に依存せず、**Git で差分管理しやすく**、  
**AI でも編集・更新しやすい構造**を目指しています。

---

# 🎯 このディレクトリの目的

この運用の目的は次のとおりです。

- 🧱 **WBS を構造化して管理する**
- 📅 **予定・実績・状態を機械可読にする**
- 📊 **Mermaid でガントチャートを可視化する**
- 📝 **背景・判断・課題を Markdown で残す**
- 🤖 **AI が更新しやすい管理構造にする**
- 🔍 **Git diff で変更履歴を追いやすくする**

---

# 🧠 基本原則

このディレクトリでは、以下を原則とします。

- **YAML = 真実のソース**
- **Mermaid = 表示用**
- **Markdown = 意味・背景・判断の記録**

つまり、

- WBS やスケジュールの実データは YAML に持つ
- ガントチャートは Mermaid で表示する
- 補足説明や課題管理は Markdown に持つ

という役割分離をします。

---

# 🗂️ ファイル構成

```text
docs/project-management/
  README.md
  ai-project-management-operating-model.md
  00_overview.md
  10_wbs.yaml
  20_schedule.yaml
  30_progress.yaml
  40_gantt_mermaid.md
  50_status_report.md
  60_risks_issues.md
```

---

# 📘 各ファイルの役割

## `README.md`
このディレクトリの入口です。  
初めて見る人向けに、構成と使い方を説明します。

## `ai-project-management-operating-model.md`
この管理方式の詳細な思想・ルール・責務分離を説明します。  
「なぜこの構造にしているのか」を知りたいときに読みます。

## `00_overview.md`
プロジェクト全体の目的・スコープ・フェーズの位置づけをまとめます。

## `10_wbs.yaml`
作業分解構造（WBS）を管理します。  
「何をやるか」の真実のソースです。

## `20_schedule.yaml`
予定・実績・状態を管理します。  
「いつやるか」「今どういう状態か」の真実のソースです。

## `30_progress.yaml`
日々の進捗事実を管理します。  
何をしたか、何が詰まっているか、次に何をするかを記録します。

## `40_gantt_mermaid.md`
Mermaid による表示用ガントチャートです。  
原則として **表示専用** とし、真実のソースにはしません。

## `50_status_report.md`
人間向けの進捗共有用ファイルです。  
週報、定例、チーム共有などに使います。

## `60_risks_issues.md`
リスク・未解決論点・懸念事項を管理します。  
ガントでは表現しにくい不確実性をここで扱います。

---

# 🔄 更新ルール

## 1. まず YAML を更新する
WBS、スケジュール、進捗事実は、まず YAML を更新します。

## 2. Mermaid は表示として更新する
`40_gantt_mermaid.md` は YAML の内容を反映するために更新します。  
Mermaid を単独で真実のソースにしないこと。

## 3. 背景説明は Markdown に書く
判断理由、課題、ブロッカー、次アクションなどは Markdown に残します。

---

# ✅ ステータス値のルール

ステータスは揺らさず、以下の固定値を使います。

```yaml
todo
doing
blocked
review
done
```

自由記述は避け、AI や人間が同じ意味で扱えるようにします。

---

# 👤 人間が主に見るファイル

普段まず見るのは次のファイルです。

- `README.md`
- `00_overview.md`
- `40_gantt_mermaid.md`
- `50_status_report.md`
- `60_risks_issues.md`

---

# 🤖 AI が主に更新しやすいファイル

AI に更新を任せやすいのは次のファイルです。

- `10_wbs.yaml`
- `20_schedule.yaml`
- `30_progress.yaml`
- `40_gantt_mermaid.md`
- `50_status_report.md`

ただし、**優先順位・完了条件・リスク判断は人間が握る**ことを前提とします。

---

# 🚦最初に読む順番

新しく参加したメンバーには、基本的に次の順で読むのがおすすめです。

1. `README.md`
2. `00_overview.md`
3. `40_gantt_mermaid.md`
4. `50_status_report.md`
5. 必要に応じて `ai-project-management-operating-model.md`

---

# 🎯 この運用の一言まとめ

> **YAML を真実のソースとし、Mermaid は可視化、Markdown は意味を持つ**

この原則に従って運用することで、  
AI と人間の両方にとって扱いやすいプロジェクト管理構造を維持します。