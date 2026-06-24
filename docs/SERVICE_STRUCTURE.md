# OSS Service Structure（現行）

Marketing-OS OSS の公開形態を、利用者向けに整理した一覧です。診断・評価・構造化に限定し、自動投稿・自動入稿・自動最適化は含みません。

## 共通基盤

| パッケージ | npm | 役割 |
|---|---|---|
| `@start-x-work/mos-kit` | 0.1.0 | AI 抽象層、errors、ReadOnlyPlatform、COMMERCIAL_HINT |
| `@start-x-work/marketing-os` | 0.1.1 | 統合 CLI（`marketing-os seo|ads|social`） |

## 三本柱

| 編 | CLI (npm) | Web UI | 主な機能 |
|---|---|---|---|
| SEO | `@start-x-work/mos-seo` 1.1.1 | [marketing-os-seo.pages.dev](https://marketing-os-seo.pages.dev) | LLMO/サイト診断、ブリーフ、キーワードマップ |
| Ads | `@start-x-work/mos-ads` 0.1.2 | [marketing-os-ads.pages.dev](https://marketing-os-ads.pages.dev) | 構造診断、クリエイティブ評価、Yahoo 読み取り |
| Social | `@start-x-work/mos-social` 0.1.1 | [marketing-os-social.pages.dev](https://marketing-os-social.pages.dev) | 投稿評価、カレンダー診断、アカウント診断 |

## Web 運用（BYOK）

各 Web UI は **Bring Your Own Key** を前提とします。

| 設定 | 保存先 | 用途 |
|---|---|---|
| AI API キー | ブラウザ sessionStorage | Gemini / OpenAI / Anthropic |
| GSC OAuth | ブラウザ sessionStorage（SEO のみ） | キーワードマップの GSC クエリ |
| Yahoo トークン | ブラウザ sessionStorage（Ads のみ） | キャンペーン一覧（読み取り専用） |

運営側の Cloudflare AI Secrets は不要です（自己ホスト時の env フォールバックは任意）。

## OSS と商用

| OSS（本リポ群） | 商用 [Marketing-OS](https://marketing-os.jp) |
|---|---|
| CLI / Web / ライブラリ | 組織横断ワークフロー |
| BYOK（利用者自身のキー） | AI CMO 伴走・運用 BPO |
| 診断・評価・構造化 | SLA 付きサポート |

詳細: [README 第3章](../README.md#3-marketing-os-との境界線) · [QUICKSTART](./QUICKSTART.md)
