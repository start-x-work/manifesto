# Phase 5 — 広告編準備 + SEO v1.0 実装指示書 v1.0

**作成日:** 2026年6月7日
**対象リポジトリ:** `start-x-work/marketing-os-seo`(v1.0化)+ `start-x-work/marketing-os-ads`(準備)
**前提:** Phase 3(CLI v0.1公開)・Phase 4(Web UI公開)が完了済み
**実行環境:** Cursor + ローカル `pnpm` / `wrangler` / `gh`
**ゴール:** SEO編を v1.0(安定版)に引き上げ、広告編の実装着手準備を完了する

---

## 0. このドキュメントについて

### 0-1. Phase 5 の二本立て

Phase 5 は性質の異なる2つの作業を並行する。

| 系統 | 対象 | 性質 |
|---|---|---|
| 系統A: SEO v1.0 | `marketing-os-seo` | 既存の安定化・拡張(実装) |
| 系統B: 広告編準備 | `marketing-os-ads` | 設計・調査(コードは最小) |

系統Aで「SEO編を完成形に固める」一方、系統Bで「広告編をいつでも着手できる状態」にする。広告編の本実装はPhase 6。

### 0-2. なぜ準備フェーズを挟むか

広告領域は技術的・規約的に最も難しい(広告API実装の重さ、プラットフォーム規約リスク)。**スコープを絞らずに着手すると失敗する。** Phase 5でスコープ・着手プラットフォーム・規約リスクを確定させてからPhase 6に入る。

---

## 1. ゴールと完了条件(Gate D)

```
Gate D 通過条件:
【系統A: SEO v1.0】
- [ ] core の公開APIインターフェースが確定(以降は破壊的変更を避ける)
- [ ] v0.2機能(キーワードボリューム推定・多言語対応)が実装済み
- [ ] LLMOスコアが複数AIモデル対応(抽象層を活用)
- [ ] v1.0.0 が npm 公開済み・Release作成済み

【系統B: 広告編準備】
- [ ] 広告編Manifestoが「Coming Soon」から「実装計画」へ更新
- [ ] 着手プラットフォームが1〜2に確定
- [ ] 規約リスク評価が完了
- [ ] SEO基盤(AI抽象層・monorepo・配布)の再利用設計が完了
```

---

## 2. 系統A: SEO v1.0

### 2-1. タスク一覧

| # | タスク | 種類 | 内容 |
|---|---|---|---|
| A1 | 公開API確定 | `[Cursor]` | core のインターフェースを固める |
| A2 | キーワードボリューム推定 | `[Cursor]` | v0.2機能 |
| A3 | 多言語対応拡張 | `[Cursor]` | 日英以外への拡張 |
| A4 | LLMOマルチモデル対応 | `[Cursor]` | 抽象層に Provider 追加 |
| A5 | Marketing-OS連携設計 | `[Cursor]` | 商用版への導線 |
| A6 | v1.0.0 リリース | `[CLI]` | npm + Release |

### A1: 公開API確定 `[Cursor]`

`packages/core` の公開インターフェースを固定する。v1.0以降は破壊的変更を避ける契約とする。

**作業:**
- `packages/core/src/index.ts` のエクスポートを整理
- 各機能の入出力型(zod スキーマ)を `types.ts` に集約
- 公開する関数・型と、内部実装(非公開)を明確に分離

```typescript
// packages/core/src/index.ts(公開API)
export {
  auditLLMO, type LLMOAuditResult, type LLMOCheck,
} from "./llmo/audit";
export {
  auditSite, type SiteAuditResult, type SiteCheck,
} from "./site/audit";
export {
  generateBrief, type ContentBrief,
} from "./content/brief";
export {
  mapKeywords, type KeywordNode, type Intent,
} from "./keyword";
export {
  type AIProvider, type CompleteOptions, createProvider,
} from "./ai";
export {
  CliError, FetchError, AIError,
} from "./errors";

// 内部実装(checks/ scoring 等)は公開しない
```

**受け入れ条件:** core が semver の安定APIを持つ。`package.json` の version を `1.0.0` に向けて準備。

### A2: キーワードボリューム推定 `[Cursor]`

v0.1で「あえて含めなかった」ボリューム推定を追加。巨大DBは持たず、GSCの実データ + 推定ロジックで実装。

```typescript
// packages/core/src/keyword/volume.ts
import type { KeywordNode } from "./index";

export interface VolumeEstimate {
  keyword: string;
  estimatedVolume: number | null;  // 推定不能なら null
  source: "gsc" | "estimated";
  confidence: "high" | "medium" | "low";
}

// GSC実データがあればそれを優先、なければクラスタ内の相対推定
export async function estimateVolume(
  nodes: KeywordNode[],
): Promise<VolumeEstimate[]> {
  // GSC impressions を直接ボリュームの代理指標に
  // GSCにないキーワードはクラスタ内の類似度から相対推定
  // ...
  return [];
}
```

**重要(思想):** 「巨大な汎用キーワードDBを持たない」というSEO Manifestoの約束は守る。Ahrefs的なDBは作らない。GSC実データ + 推定にとどめる。

**受け入れ条件:** `mos-seo keyword map <seed> --volume` でボリューム推定が付く。GSC連携時は実データ、なければ推定+confidence表示。

### A3: 多言語対応拡張 `[Cursor]`

v0.1の日英から、対応言語を拡張。

**作業:**
- LLMO診断・コンテンツブリーフのプロンプトを言語パラメータ化
- `--lang` オプション追加(`mos-seo content brief <topic> --lang en`)
- 出力(推奨・ブリーフ)を指定言語で生成

```typescript
// 言語をオプション化
export interface BriefOptions {
  lang?: string; // ISO 639-1: "ja" | "en" | "zh" | ...
}
export async function generateBrief(
  ai: AIProvider, topic: string, opts: BriefOptions = {},
): Promise<ContentBrief> {
  const lang = opts.lang ?? "ja";
  // プロンプトに言語指定を含める
}
```

**受け入れ条件:** `--lang` で出力言語が変わる。デフォルトは ja。

### A4: LLMOマルチモデル対応 `[Cursor]`

AI抽象層を活かし、Gemini以外のProviderを追加。LLMO診断を複数モデルで検証できるようにする。

```typescript
// packages/core/src/ai/openai.ts(新規)
import type { AIProvider, CompleteOptions } from "./provider";

export class OpenAIProvider implements AIProvider {
  // OpenAI API 実装
}

// packages/core/src/ai/anthropic.ts(新規、任意)
export class AnthropicProvider implements AIProvider {
  // Anthropic API 実装
}

// createProvider をモデル選択可能に
export function createProvider(
  model: "gemini" | "openai" | "anthropic" = "gemini",
  apiKey?: string,
): AIProvider {
  switch (model) {
    case "openai": return new OpenAIProvider(apiKey);
    case "anthropic": return new AnthropicProvider(apiKey);
    default: return new GeminiProvider(apiKey);
  }
}
```

**受け入れ条件:** `--model gemini|openai` でLLMO診断のAIモデルを切り替えられる。抽象層が機能している証明。

### A5: Marketing-OS連携設計 `[Cursor]`

OSS版から商用Marketing-OSへの導線を設計(コードではなく、出力への自然な誘導)。

**作業:**
- 診断結果の末尾に、商用版でできること(実行支援・継続モニタリング)への言及を**控えめに**追加
- 押し付けない。「この診断を継続的に・チームで運用するなら marketing-os.jp」程度
- Web UI(Phase 4)のフッターにも同様の導線

**受け入れ条件:** OSS→商用への導線が、思想を損なわない範囲で存在する。

### A6: v1.0.0 リリース `[CLI]`

```bash
cd ~/projects/marketing-os-seo
pnpm lint && pnpm build && pnpm test

# version を 1.0.0 に
# packages/cli/package.json と core を更新

cd packages/cli && npm publish

cd ~/projects/marketing-os-seo
git tag v1.0.0 && git push origin v1.0.0
gh release create v1.0.0 --repo start-x-work/marketing-os-seo \
  --title "SEO Toolkit v1.0.0" \
  --notes "Stable release. Adds keyword volume estimation, multi-language support, multi-model LLMO. Stable public API."
```

**受け入れ条件:** v1.0.0 が npm + Release で公開。

---

## 3. 系統B: 広告編準備

### 3-1. タスク一覧

| # | タスク | 種類 | 内容 |
|---|---|---|---|
| B1 | 広告API調査 | `[Cursor]` + `[手動]` | 6プラットフォームの実装コスト評価 |
| B2 | 着手プラットフォーム確定 | `[手動]` | 1〜2に絞る |
| B3 | 規約リスク評価 | `[Cursor]` + `[手動]` | 自動化ポリシー確認 |
| B4 | スコープ確定 | `[手動]` | 作る/作らないの線引き |
| B5 | 広告編Manifesto更新 | `[Cursor]` | Coming Soon → 実装計画 |
| B6 | 再利用設計 | `[Cursor]` | SEO基盤の流用方法 |

### B1: 広告API調査 `[Cursor]` + `[手動]`

各プラットフォームの実装コストを評価し、`marketing-os-ads/docs/api-research.md` に記録。

| プラットフォーム | 認証 | 実装コスト | 規約リスク | 調査項目 |
|---|---|---|---|---|
| Google Ads | OAuth2 + Developer Token | 高(GAQL) | 中 | 自動化ポリシー、Developer Token審査 |
| Meta Ads | OAuth2 + Business Manager | 高(頻繁変更) | 中 | アプリ審査、データ利用規約 |
| Yahoo!広告 | OAuth2 | 中 | 低 | 日本独自仕様 |
| X Ads | OAuth2 | 中 | 高 | API制限・有料化動向 |
| LINE Ads | OAuth2 + LINE Business | 中 | 低 | 日本独自 |
| TikTok Ads | OAuth2 | 中 | 中 | ドキュメント品質 |

**Cursorの作業:** 各プラットフォームの公式APIドキュメントを調査し、認証フロー・主要エンドポイント・レート制限・自動化に関する規約条項を要約。

**手動の作業:** 各プラットフォームのDeveloper規約を最終確認(法的判断は人間が行う)。

### B2: 着手プラットフォーム確定 `[手動]`

調査結果から、**Phase 6で着手する1〜2プラットフォーム**を決める。

推奨判断軸:
- 実装コストが低い(Yahoo!・LINEは日本市場で有利かつ規約リスク低)
- 規約リスクが低い
- ターゲット(日本のマーケ責任者)の利用率が高い

**この判断はユーザー(山口偉大)が行う。** Cursorは判断材料を提示するのみ。

### B3: 規約リスク評価 `[Cursor]` + `[手動]`

各プラットフォームの「自動化・サードパーティツール」に関する規約条項を整理。

**重要な観点:**
- 自動運用ツールがアカウント停止リスクを生まないか
- OSS利用者が規約違反に問われないか
- 「診断・評価・構造化」にとどめれば回避できるか(おそらくYes)

**結論の方向性(予測):** 「自動入稿・自動運用」は規約リスクが高い。「キャンペーン設計の構造化・配信判断のログ化・クリエイティブ評価」にとどめれば、リスクは大幅に下がる。これはMarketing-OS思想(診断・評価まで)とも一致する。

### B4: スコープ確定 `[手動]`

広告編 v0.1 の「作る/作らない」を確定。

| 候補機能 | v0.1判断(予測) | 理由 |
|---|---|---|
| キャンペーン設計・配信判断のログ構造化 | ★ 作る | 意思決定支援、規約リスク低 |
| クリエイティブ評価の再現可能な手続き | ★ 作る | 評価であって生成ではない |
| 広告プラットフォームAPI統合(読み取り) | △ 限定的に | データ取得のみ、書き込みなし |
| 自動運用ロジック | ✗ 作らない | 規約リスク高、思想に反する |
| 自動入稿 | ✗ 作らない | 同上 |
| クリエイティブ自動生成 | ✗ 作らない | 競合過多、思想に反する |

### B5: 広告編Manifesto更新 `[Cursor]`

`manifesto/ads/README.md` を「Coming Soon」から「実装計画」へ更新。

**作業:**
- B1〜B4の確定事項を反映
- 着手プラットフォーム、スコープ、「作らないもの」を明記
- リリース予定を実進行に合わせて更新
- 日英併記を維持

**受け入れ条件:** 広告編Manifestoが、実装に着手できる具体性を持つ。

### B6: 再利用設計 `[Cursor]`

SEO編で確立した基盤を広告編でどう流用するか設計。`marketing-os-ads/docs/architecture.md` に記録。

| SEO編の資産 | 広告編での再利用 |
|---|---|
| AI抽象層(`ai/provider.ts`) | そのまま流用 |
| monorepo構成(core/cli/web) | 同一構成で初期化 |
| 出力整形(json/table/markdown) | 流用 |
| エラー型(CliError等) | 流用 |
| CI(GitHub Actions) | 流用 |
| 配布(npm scoped) | `@start-x-work/mos-ads` |

**受け入れ条件:** 広告編が「SEO編のコピーから始められる」設計図がある。

---

## 4. Definition of Done(Gate D)

### 系統A
- [ ] core 公開API確定(A1)
- [ ] ボリューム推定実装(A2)
- [ ] 多言語対応(A3)
- [ ] マルチモデルLLMO(A4)
- [ ] Marketing-OS連携導線(A5)
- [ ] v1.0.0 公開(A6)

### 系統B
- [ ] 広告API調査完了(B1)
- [ ] 着手プラットフォーム確定(B2)
- [ ] 規約リスク評価完了(B3)
- [ ] スコープ確定(B4)
- [ ] 広告編Manifesto更新(B5)
- [ ] 再利用設計完了(B6)

すべて満たせば **Gate D 通過 → Phase 6(広告編実装)着手可能**。

---

## 5. やらないこと

- 広告の自動運用・自動入稿(規約リスク + 思想違反)
- クリエイティブ自動生成(競合過多 + 思想違反)
- 巨大キーワードDBの構築(SEO Manifestoの約束)
- 6プラットフォーム同時着手(スコープ過大)

---

## 6. Cursor Agent への指示(コピペ用)

```
@phase5_ads_prep_seo_v1.md に従い、Phase 5 を実行します。

系統A(SEO v1.0)と系統B(広告編準備)を並行します。

系統A: A1(公開API確定)から。core のインターフェースを固め、
v0.2機能(A2ボリューム推定・A3多言語・A4マルチモデル)を実装。
- 巨大キーワードDBは作らない(GSC実データ+推定まで)
- AI抽象層を活かしてマルチモデル対応
A6(v1.0公開)は私が確認してから。

系統B: B1(広告API調査)から。各プラットフォームの公式ドキュメントを調査し、
docs/api-research.md にまとめる。B2(着手プラットフォーム確定)・B3(規約リスク)・
B4(スコープ確定)は私が判断するので、判断材料を提示してください。
B5(Manifesto更新)・B6(再利用設計)は私の判断確定後に。

規約の法的判断は私が行います。あなたは事実の調査・整理まで。
各タスクの受け入れ条件を満たしてから次へ。各段階で commit。
```

---

## 7. Phase 6 への接続

Gate D 通過後、Phase 6(広告編 v0.1実装)へ。実装指示書は `phase6_ads_impl_v1.md`(別ファイル)。

---

## 8. バージョン管理

- v1.0: 2026年6月7日 初版
