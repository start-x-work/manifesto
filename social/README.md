# Marketing-OS Manifesto — SNS 編 / Social pillar

## 日本語

### 問題提起

ソーシャル向けツールは投稿効率化に偏りがちである。スケジューラ、承認フロー、ダッシュボードは成熟した一方で、「コミュニティの声をどの意思決定に接続するか」という編集問題は依然として人間の暗黙知に寄りかかりやすい。運用は速くなっても、場のルールと優先順位の言語化が追いつかない。

### v0.1 の位置づけ（2026年 — 前倒し着手）

SEO 編・広告編・mos-kit 共通基盤が整ったため、SNS 編 v0.1 の実装を開始した。CLI `@start-x-work/mos-social` として、評価・診断・構造化に限定する。**自動投稿は interface 設計で構造的に排除**する。

### v0.1 で作るもの

1. **投稿評価** — プラットフォーム別の評価手続き（生成はしない）
2. **コンテンツカレンダー診断** — 1/10 ルール（販促比率）等の Content OS 知見
3. **アカウント/プロフィール診断** — 読み取り可能な範囲での監査
4. **手動エクスポート読み込み** — API 制約のあるプラットフォーム向け

実装リポジトリ: [marketing-os-social](https://github.com/start-x-work/marketing-os-social)  
Web UI: [marketing-os-social.pages.dev](https://marketing-os-social.pages.dev)  
**クイックスタート:** [docs/QUICKSTART.md](https://github.com/start-x-work/marketing-os-social/blob/main/docs/QUICKSTART.md)

### あえて作らないもの（v0.1）

自動投稿、スケジュール投稿、一括 publish、削除 API。理由は Marketing-OS 思想（Part 9: 自動投稿は永続的に禁止）と、プラットフォーム規約リスク。

### 他編との関係

SEO 編がオーガニックの可視性を、広告編が有料の判断ログを扱う。SNS 編は、パブリックな対話の素材を意思決定に再接続する。三編は `@start-x-work/mos-kit` で共通基盤を共有する。

---

## English

### The gap

Social tooling skews toward posting efficiency. Schedulers and dashboards are mature, yet **which community signals connect to which decisions** still leans on tacit knowledge.

### v0.1 status (2026 — started ahead of original schedule)

With SEO, Ads, and `@start-x-work/mos-kit` in place, Social v0.1 shipped as CLI `@start-x-work/mos-social`, limited to evaluation and diagnosis. **Auto posting is structurally excluded** from the platform interface.

### What v0.1 ships

1. **Post evaluation** — platform-aware procedures (not generation)
2. **Content calendar diagnosis** — Content OS rules such as the 1/10 promotional ratio
3. **Account/profile audit** — within read-only bounds
4. **Manual export ingestion** — for API-constrained platforms

Repository: [marketing-os-social](https://github.com/start-x-work/marketing-os-social)

### What v0.1 deliberately excludes

Auto posting, scheduled publishing, bulk publish, delete APIs — aligned with Marketing-OS policy and platform risk.

### Relation to other pillars

SEO handles organic visibility; Ads handles paid decision logs; Social reconnects public conversation to decisions. All three share `@start-x-work/mos-kit`.
