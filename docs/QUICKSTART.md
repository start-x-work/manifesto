# Quickstart — Marketing-OS OSS

3 編 CLI + Web UI の利用者向け入口です。

## 最短パス

| やりたいこと | コマンド / URL |
|---|---|
| サイト診断 | `npx @start-x-work/mos-seo audit site https://example.com` |
| 広告構造診断 | `npx @start-x-work/mos-ads campaign analyze '<json>'` |
| 投稿評価 | `npx @start-x-work/mos-social post evaluate "text" --platform x` |
| 3 編まとめて | `npx @start-x-work/marketing-os seo|ads|social ...` |

## Web UI（BYOK）

| 編 | URL | 設定 |
|---|---|---|
| SEO | https://marketing-os-seo.pages.dev | AI キー + GSC OAuth（任意） |
| Ads | https://marketing-os-ads.pages.dev | AI キー + Yahoo トークン（任意） |
| Social | https://marketing-os-social.pages.dev | AI キー |

**BYOK（Bring Your Own Key）:** API キー・OAuth 認証情報はブラウザの sessionStorage にのみ保存。運営側の Cloudflare Secrets は不要（自己ホスト時の env フォールバックは任意）。

## 各編の詳細手順

- [SEO Quickstart](https://github.com/start-x-work/marketing-os-seo/blob/main/docs/QUICKSTART.md)
- [Ads Quickstart](https://github.com/start-x-work/marketing-os-ads/blob/main/docs/QUICKSTART.md)
- [Social Quickstart](https://github.com/start-x-work/marketing-os-social/blob/main/docs/QUICKSTART.md)
- [Unified CLI Quickstart](https://github.com/start-x-work/marketing-os/blob/main/docs/QUICKSTART.md)

現行サービス構成: [SERVICE_STRUCTURE.md](./SERVICE_STRUCTURE.md)

## OSS と商用 Marketing-OS

- **OSS:** 診断・評価・CLI/Web/ライブラリ — フォーク・組み込み可
- **商用:** 意思決定 OS としての統合体験、ワークフロー、SLA — [marketing-os.jp](https://marketing-os.jp)

[Manifesto — 境界線](./README.md#3-marketing-os-との境界線)
