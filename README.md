# Research OS ドキュメント

このリポジトリは、**研究開発チームの Operating System（Research OS）** に関するドキュメントをまとめたものです。

> 思想・導入ガイド・プロジェクト管理の指針を一箇所に集約し、  
> チーム横断で参照・更新できるようにするためのナレッジベースです。

---

## 概要

Research OS は、単なる Knowledge Base や Handbook ではなく、

- **思想 (Philosophy)** と **研究戦略**
- **運用ルール** と **意思決定履歴**
- **プロジェクト管理** の考え方とテンプレート

を統合した「研究開発組織の基盤」として設計されています。

- **Docs as Code**: Markdown を Git で管理し、PR でレビュー・差分追跡
- **AI フレンドリー**: テキストベースで AI が編集・更新しやすい構成
- **一元的な入口**: 新メンバーもブラウザから参照しやすい構成

---

## 目次（ナビゲーション）

### 基本ドキュメント

| ドキュメント | 説明 |
|-------------|------|
| [Research OS 思想](./docs/others/research-os/00_research-os-philosophy.md) | Research OS の考え方・なぜ必要か・何を統合するか |
| [Research OS 導入ガイド](./docs/others/research-os/01_research-os_bootstrap_guide.md) | GitHub + Markdown + MkDocs + GitHub Pages による Docs-as-Code 基盤の構築手順 |

### プロジェクト管理

| ドキュメント | 説明 |
|-------------|------|
| [Project Management 概要](./docs/others/research-os/project-management/README.md) | YAML + Mermaid + Markdown によるプロジェクト管理の目的とファイル構成 |
| [プロジェクト概要（iTenFs）](./docs/others/research-os/project-management/00_overview.md) | iTenFs 開発プロジェクトの目的・フェーズ構成・PR 概要 |
| [WBS YAML スキーマ設計](./docs/others/research-os/project-management/10_wbs.md) | AI 管理用に最適化した WBS 設計と `10_wbs.yaml` の考え方 |
| [ガントチャート](./docs/others/research-os/project-management/40_gantt.md) | iTenFs プロジェクトの Mermaid ガントチャート |
| [AI Project Management 運用モデル](./docs/others/research-os/project-management/ai-project-management-operating-model.md) | Mermaid + YAML + Markdown による運用ガイドと管理対象の分離 |

---

## クイックリンク

- [思想・哲学](./docs/others/research-os/00_research-os-philosophy.md) — Research OS とは何か
- [導入ガイド](./docs/others/research-os/01_research-os_bootstrap_guide.md) — 環境構築・リポジトリ方針
- [プロジェクト管理トップ](./docs/others/research-os/project-management/README.md) — WBS・スケジュール・ガントの入口

---

## このリポジトリの構成

```
docs/others/research-os/
├── 00_research-os-philosophy.md      # 思想
├── 01_research-os_bootstrap_guide.md # 導入ガイド
└── project-management/               # プロジェクト管理
    ├── README.md
    ├── 00_overview.md
    ├── 10_wbs.md
    ├── 20_schedule.yaml
    ├── 30_progress.yaml
    ├── 40_gantt.md
    └── ai-project-management-operating-model.md
```

YAML（`10_wbs.yaml`, `20_schedule.yaml`, `30_progress.yaml`）はデータの真実のソース、  
Markdown は説明・背景・判断の記録、Mermaid は可視化用として役割を分けています。
