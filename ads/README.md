# Marketing-OS Manifesto — 広告編 / Ads pillar

## 日本語

### 問題提起

広告運用は、内製化と代理店依存のあいだで常に引き裂かれる。内製は速度と文脈の理解に強いが、プラットフォーム差分と学習コストが重い。代理店は専門性とスケールを提供するが、意思決定のログが外部に偏り、組織の編集権が弱まることがある。ツールは増えたが、「なぜその予算配分か」を組織内で再編集し続ける構造は依然として不足しがちである。

### v0.1 の位置づけ（2026年 — 前倒し着手）

SEO 編・mos-kit 共通基盤が整ったため、広告編 v0.1 の実装を開始する。CLI `@start-x-work/mos-ads` として、診断・評価・ログ構造化に限定する。

### v0.1 で作るもの

1. **キャンペーン構造診断** — 設計上の問題点を診断する（実行・入稿はしない）
2. **配信判断ログ** — 人間の判断を構造化して記録する
3. **クリエイティブ評価** — 再現可能な評価手続き（生成はしない）
4. **プラットフォーム読み取り（限定）** — `@start-x-work/mos-kit` の `ReadOnlyPlatform` 経由。第一候補: Yahoo! / LY Ads

実装リポジトリ: [marketing-os-ads](https://github.com/start-x-work/marketing-os-ads)  
調査・設計: [api-research.md](https://github.com/start-x-work/marketing-os-ads/blob/main/docs/api-research.md) · [architecture.md](https://github.com/start-x-work/marketing-os-ads/blob/main/docs/architecture.md)

### あえて作らないもの（v0.1）

自動入稿、自動予算変更、自動運用ルール、クリエイティブ自動生成。理由は Marketing-OS 思想（意思決定支援まで）と、プラットフォーム規約リスクの両方。

### 戦略的意図

広告編は SEO 編の延長線上にある。共通基盤 `@start-x-work/mos-kit` により、AI 抽象層と読み取り専用プラットフォーム境界を3編で共有する。

---

## English

### The tension

Ad operations oscillate between in-house control and agency dependence. In-house teams move fast and understand context, but face heavy platform deltas and learning costs. Agencies provide expertise and scale, yet decision logs drift outward, weakening the organization’s editorial authority.

### v0.1 status (2026 — started ahead of original schedule)

With the SEO pillar and `@start-x-work/mos-kit` in place, Ads v0.1 implementation has started as CLI `@start-x-work/mos-ads`, limited to diagnosis, evaluation, and structured decision logs.

### What v0.1 ships

1. **Campaign structure diagnosis** — surface design issues without executing changes
2. **Delivery decision logs** — structure human judgments for reuse
3. **Creative evaluation** — repeatable evaluation procedures (not generation)
4. **Limited platform read access** — via mos-kit `ReadOnlyPlatform`; first candidate: Yahoo! / LY Ads

Repository: [marketing-os-ads](https://github.com/start-x-work/marketing-os-ads)

### What v0.1 intentionally skips

Auto submission, auto budget changes, auto optimization rules, and auto creative generation — aligned with Marketing-OS boundaries and platform policy risk.

### Strategic intent

The Ads pillar extends the SEO pillar. Shared kit keeps AI abstraction and read-only platform boundaries consistent across pillars.
