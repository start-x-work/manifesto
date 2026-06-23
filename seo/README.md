# Marketing-OS Manifesto — SEO 編 / SEO pillar

## 日本語

### 業界の空白地帯

検索エンジン最適化の知見は成熟している一方で、ChatGPT・Perplexity・Google AI Overviews など、生成 AI を介した可視性（LLMO）や、回答最適化（AEO）の実務はまだ揺れている。ベンダーはスコアやレポートを提供するが、診断の前提・プロンプト設計・再現手続きがブラックボックスのまま流通しやすい。結果として、チーム内で「なぜその優先順位か」を編集し続ける土台が薄い。

### v0.1（CLI + Web UI、2026 年 6 月公開済み）

次の四機能を中核とする。CLI は [@start-x-work/mos-seo](https://www.npmjs.com/package/@start-x-work/mos-seo)、Web UI は [marketing-os-seo.pages.dev](https://marketing-os-seo.pages.dev) から利用できる。実装リポジトリ: [marketing-os-seo](https://github.com/start-x-work/marketing-os-seo)

**Web UI:** [marketing-os-seo.pages.dev](https://marketing-os-seo.pages.dev) — AI キーと GSC OAuth は **BYOK**（ブラウザ sessionStorage）。  
**クイックスタート:** [docs/QUICKSTART.md](https://github.com/start-x-work/marketing-os-seo/blob/main/docs/QUICKSTART.md)。

1. **LLMO／AEO 診断** — 生成 AI 経由の可視性を、再現可能な手順とスコア枠で把握する。
2. **サイト診断・内部対策** — 技術的 SEO の主要項目（構造化データ、サイトマップ、robots、CWV 関連など）を一括で扱う。
3. **コンテンツ制作支援** — ブリーフとプロンプト設計を、編集可能なテンプレートとして出力する。
4. **キーワード調査（コア）** — 意図分類とクラスタリング、GSC 連携を前提に、検索意図の地図を作る。

### あえて作らないもの（v0.1）

順位チェックの日次トラッキング、巨大な汎用キーワード DB、競合の全自動クロールに依存した評価は v0.1 では扱わない。理由は、データ所有とコスト構造がロックインを生みやすく、OSS としての検証可能性を損ないやすいからである。ロードマップ上、一部は v0.2 以降で再検討する。

### 技術選択の理由

**TypeScript** は型によりインタフェース契約を共有しやすく、CLI と将来の Web UI を同一言語で接続しやすい。**Cloudflare**（Workers／Pages／関連ストア）は、エッジでの実行と配布を単純化し、利用者が自分の環境に近い形で試せる。**Gemini** は現行プロダクトとの整合と、マルチモーダル文脈での診断表現の自由度を考慮して採用する。モデル固定は恒久方針ではない。抽象層を挟み、将来の差し替えを妨げない設計にする。

### 商用との関係

SEO 編 OSS は素材と診断枠を開く。組織全体のワークフロー、SLA、AI CMO 的な伴走は商用 Marketing-OS 側に残す。併用は推奨されるが必須ではない。

---

## English

### The gap

Classic SEO is well documented, yet visibility through generative surfaces (LLMO) and answer-oriented optimization (AEO) still shifts in practice. Vendors ship scores and reports, but premises, prompt design, and reproducible procedure often circulate as black boxes. Teams then lack a durable base for continuously editing “why this priority.”

### What v0.1 ships (CLI + Web UI, published June 2026)

Four pillars, available via CLI ([@start-x-work/mos-seo](https://www.npmjs.com/package/@start-x-work/mos-seo)) and Web UI ([marketing-os-seo.pages.dev](https://marketing-os-seo.pages.dev)). Repository: [marketing-os-seo](https://github.com/start-x-work/marketing-os-seo).

1. **LLMO / AEO audit** — capture generative visibility with reproducible steps and a scoring frame.
2. **Technical SEO audit** — cover major technical items (structured data, sitemap, robots, CWV-related signals, and related checks).
3. **Content brief support** — emit briefs and prompt patterns as editable templates.
4. **Keyword research (core)** — intent mapping and clustering with Google Search Console integration in mind.

### What v0.1 intentionally skips

Daily rank tracking, giant generic keyword databases, and competitor evaluation that depends on opaque bulk crawling are out of scope for v0.1—they tend to couple cost and data ownership in ways that invite lock-in and weaken verifiability in OSS. Some may return on the roadmap for later versions.

### Why TypeScript, Cloudflare, and Gemini

**TypeScript** shares interface contracts cleanly across CLI and a future web UI. **Cloudflare** simplifies edge execution and distribution so adopters can try flows close to their own environments. **Gemini** aligns with current product practice and offers flexibility for diagnostic narratives across modalities. Model choice is not permanent; we will insert an abstraction layer so swaps remain feasible.

### Relation to commercial

The SEO OSS pillar opens material and diagnostic scaffolds. Organization-wide workflows, SLAs, and AI CMO-style accompaniment stay in commercial Marketing-OS. Using both is welcome, not mandatory.
