# Start-X OSS 実装ブループリント v2.0 — 最速完成・網羅版

**作成日:** 2026年6月7日
**前提:** これは「いつやるか(時系列・ロードマップ)」を扱わない。**「何を・どの依存順で・どう作るか」を最大精度で**定義する実装ブループリント。
**最速化の原理:** 時間軸ではなく **依存グラフ + 並列化** で総所要を最小化する。依存のないノードはすべて同時着手可能。
**対象:** `start-x-work` Organization 全リポジトリ
**実行:** Cursor(Agent)+ ローカル `pnpm` / `npm` / `wrangler` / `gh`
**唯一の時間制約:** 運用マニュアル Part 9(週20h)。並列化は稼働増ではなく「待ち時間ゼロ化」の意味。

---

## 0. 設計思想:最速とは何か

### 0-1. 最速 = クリティカルパスの最小化

総完成時間は「最も長い依存連鎖(クリティカルパス)」で決まる。日付を切るのではなく、**依存のない作業を全部並列に倒す**ことで最速化する。本書は各ノードの依存を厳密に定義し、並列実行可能な集合を明示する。

### 0-2. 精度 = 着手即実装できる粒度

各ノードは「型定義・スキーマ・受け入れ条件・検証コマンド・Cursorプロンプト」を持つ。Cursorに渡せばそのまま実装が始まる。曖昧さを残さない。

### 0-3. 構想の最終形(完成の定義)

```
3カテゴリ(SEO/広告/SNS)が
  × 2形態(CLI + Web)で揃い
  × 共通基盤(mos-kit)で重複ゼロ
  × 統合CLI(marketing-os)で束ねられ
  × 商用Marketing-OSへの導線を持ち
  × すべて「診断・評価まで」の境界を構造的に守る
状態。
```

---

## 1. 現在地(GitHub事実・2026-06-07)

| リポ | 実体 | 残 |
|---|---|---|
| manifesto | 公開済み | — |
| marketing-os-seo | CLI 4機能コード有・3コミット | **npm未公開・Release無** |
| marketing-os-ads | scaffold(Coming表記) | 全実装 |
| marketing-os-social | scaffold(Coming表記) | 全実装 |
| mos-kit | 未作成 | 新設 |
| marketing-os(統合) | 未作成 | 新設 |

---

## 2. 依存グラフ(これが本書の中核)

```
                    ┌─────────────────────────────────────┐
                    │  N0  SEO CLI 公開(npm + Release)     │  ← 唯一の即着手ノード
                    └───────────────┬─────────────────────┘
                                    │
          ┌─────────────────────────┼──────────────────────────┐
          │                         │                          │
          ▼                         ▼                          ▼
┌──────────────────┐   ┌────────────────────┐   ┌──────────────────────┐
│ N1 SEO v1.0       │   │ N2 SEO Web UI       │   │ N3 広告API調査・規約  │
│ (安定API/v0.2機能) │   │ (Cloudflare)        │   │ (机上・コード不要)    │ ← N0と無関係に並列可
└────────┬─────────┘   └─────────┬──────────┘   └──────────┬───────────┘
         │                       │                          │
         └───────────┬───────────┘                          │
                     ▼                                       │
          ┌────────────────────┐                            │
          │ N4 mos-kit 抽出     │ ← N1完了(安定API)が前提     │
          │ (共通基盤/読取専用基底)│                            │
          └─────────┬──────────┘                            │
                    │                                        │
        ┌───────────┼────────────────────┬──────────────────┘
        ▼           ▼                    ▼
┌──────────────┐ ┌──────────────┐  (N3の確定が N5 に合流)
│ N5 広告 CLI   │ │ N6 SNS CLI    │
│ (mos-kit利用) │ │ (mos-kit利用) │  ← N4後、互いに並列可
└──────┬───────┘ └──────┬───────┘
       │                │
       ▼                ▼
┌──────────────┐ ┌──────────────┐
│ N7 広告 Web   │ │ N8 SNS Web    │  ← 各CLI後、並列可
└──────┬───────┘ └──────┬───────┘
       └────────┬───────┘
                ▼
       ┌─────────────────┐
       │ N9 統合CLI       │ ← 3編CLI(N5,N6,SEO)が前提
       │ marketing-os    │
       └────────┬────────┘
                ▼
       ┌─────────────────┐
       │ N10 商用導線      │ ← 随時。どのノードとも並列可
       └─────────────────┘
```

### 2-1. クリティカルパス(最長依存連鎖)

```
N0 → N1 → N4 → N5(or N6) → N7(or N8) → N9
```

これが最速完成の下限。**この連鎖上にないノード(N2, N3, N10)は、連鎖と完全並列**で消化する。

### 2-2. 並列実行集合(待ち時間ゼロ化)

| ウェーブ | 同時着手可能なノード | 前提 |
|---|---|---|
| W1 | **N0** | なし(即) |
| W2 | **N1, N2, N3** | N0完了 |
| W3 | **N4** | N1完了 |
| W4 | **N5, N6** | N4完了(N5はN3の確定も使う) |
| W5 | **N7, N8** | 各CLI(N5/N6)完了 |
| W6 | **N9** | N5,N6,SEO CLI完了 |
| 随時 | **N10** | いつでも |

W2でN3(広告調査=コード不要の机上作業)を並列消化しておくと、W4の広告CLI着手時に待ちが出ない。これが最速化の肝。

---

## 3. 全ノード実装仕様

各ノード: 目的 / 依存 / 成果物 / 実装核(型・スキーマ・コード) / 受け入れ条件 / 検証 / Cursorプロンプト。

---

### N0 — SEO CLI 公開

**目的:** コードのある SEO編を npm公開 + Release し客観的に公開状態に。
**依存:** なし(即着手)
**成果物:** `@start-x-work/mos-seo` v0.1.0(npm)、GitHub Release

**実装核:** cli の tsup で core をバンドル(単一パッケージ配布)。

```typescript
// packages/cli/tsup.config.ts
import { defineConfig } from "tsup";
export default defineConfig({
  entry: ["src/index.ts"],
  format: ["esm"],
  clean: true,
  shims: true,
  noExternal: [/@start-x-work\/.*/],   // coreをバンドル
});
```

**検証:**

```bash
cd ~/projects/marketing-os-seo
git push origin main
pnpm install --frozen-lockfile && pnpm lint && pnpm build && pnpm test
cd packages/cli && npm publish
cd ~/projects/marketing-os-seo && git tag v0.1.0 && git push origin v0.1.0
gh release create v0.1.0 --title "SEO Toolkit v0.1.0 (CLI)" --notes "LLMO/site audit, content brief, keyword mapper."
npx @start-x-work/mos-seo audit site https://example.com --format json
```

**受け入れ:** [ ] npx で外部動作 [ ] Release存在 [ ] CI緑

**Cursorプロンプト:**

```
marketing-os-seo を公開可能状態にする。lint/build/test を緑にし、CIワークフロー(.github/workflows/ci.yml: pnpm install→lint→build→test, Node22, テストはAIモックで実APIキー不要)を追加。tsup の noExternal で core を cli にバンドル。npm publish と Release は私が実行するのでコマンドを提示。
```

---

### N1 — SEO v1.0(安定API + v0.2機能)

**目的:** core を semver安定APIにし、ボリューム推定・多言語・マルチモデルを追加。**N4(mos-kit抽出)の前提。**
**依存:** N0
**成果物:** `@start-x-work/mos-seo` v1.0.0

**実装核:**

```typescript
// 公開API確定(packages/core/src/index.ts)
export { auditLLMO, type LLMOAuditResult, type LLMOCheck } from "./llmo/audit";
export { auditSite, type SiteAuditResult, type SiteCheck } from "./site/audit";
export { generateBrief, type ContentBrief, type BriefOptions } from "./content/brief";
export { mapKeywords, type KeywordNode, type Intent } from "./keyword";
export { estimateVolume, type VolumeEstimate } from "./keyword/volume";
export { type AIProvider, type CompleteOptions, createProvider } from "./ai";
export { CliError, FetchError, AIError } from "./errors";

// ボリューム推定(巨大DBは作らない:GSC実データ + クラスタ相対推定)
export interface VolumeEstimate {
  keyword: string;
  estimatedVolume: number | null;
  source: "gsc" | "estimated";
  confidence: "high" | "medium" | "low";
}

// マルチモデル(抽象層にProvider追加)
export function createProvider(
  model: "gemini" | "openai" | "anthropic" = "gemini",
  apiKey?: string,
): AIProvider { /* switch */ }

// 多言語(オプション化)
export interface BriefOptions { lang?: string } // "ja" default
```

**受け入れ:** [ ] 公開API固定(以降破壊的変更なし) [ ] `keyword map --volume` 動作 [ ] `--lang` 動作 [ ] `--model` 動作 [ ] v1.0.0公開

**Cursorプロンプト:**

```
marketing-os-seo を v1.0 化。core/src/index.ts に公開APIを集約し semver安定に。
ボリューム推定(volume.ts: GSC実データ優先・なければクラスタ相対推定+confidence、巨大DBは作らない)、
多言語(--lang、default ja)、マルチモデル(createProvider に openai/anthropic 追加、--model 切替)を実装。
公開は私が実行。
```

---

### N2 — SEO Web UI

**目的:** CLI 4機能を Web 化。**core を書き換えず再利用**(Workers対応の後方互換調整のみ)。
**依存:** N0(coreが公開済みであればよい。N1完了は不要 → N1と並列可)
**成果物:** Cloudflare Pages 上の SEO Web

**実装核:**

```typescript
// core 後方互換調整(Workersは process.env 不可)
export function createProvider(model = "gemini", apiKey?: string): AIProvider {
  return buildProvider(model, apiKey ?? globalThis.process?.env?.GEMINI_API_KEY);
}

// functions/api/audit/llmo.ts (Cloudflare Pages Function)
import { auditLLMO } from "@start-x-work/marketing-os-seo-core";
export const onRequestGet: PagesFunction<{GEMINI_API_KEY:string}> = async (ctx) => {
  const url = new URL(ctx.request.url).searchParams.get("url");
  if (!url) return Response.json({ error: "url required" }, { status: 400 });
  try { return Response.json(await auditLLMO(url)); }
  catch (e) { return Response.json({ error: String(e) }, { status: 500 }); }
};
```

**デザイントークン(全Web共通・全編で再利用):**

```typescript
export const colors = {
  indigo: "#5957EE", slate: "#0A2540", white: "#FFFFFF",
  indigoLight: "#EEEEFE", slateMuted: "#425466", border: "#E6E9EC",
  success: "#1A9D62", warning: "#C77700", danger: "#DF1B41",
} as const;
// fonts: Manrope + Noto Sans JP, weight-300ベース, カードレイアウト
```

**スタック:** React18 + Vite + Tailwind + React Router + TanStack Query + Cloudflare Workers/Pages + Google OAuth(GSC) + Biome。

**受け入れ:** [ ] 4機能Web動作 [ ] GSC OAuth動作 [ ] Pages公開 [ ] coreは後方互換調整のみ

**Cursorプロンプト:**

```
@phase4_seo_web_impl_v1.md に従い packages/web 実装。core は書き換えず createProvider の
apiKey引数追加(後方互換)のみ。React+Vite+Tailwind+Workers、Google OAuth(GSC)、
Stripe DESIGN(OS Indigo #5957EE)。OAuth/secret設定は私が手動。
```

---

### N3 — 広告API調査・規約評価(コード不要・W2で並列消化)

**目的:** 広告編着手時の待ちをゼロにするため、机上調査を先行。**N0と無関係に着手可能。**
**依存:** なし(実質いつでも。W2で並列推奨)
**成果物:** `marketing-os-ads/docs/api-research.md`、着手プラットフォーム確定、スコープ確定

**調査マトリクス:**

| PF | 認証 | 実装コスト | 規約リスク | 読取API |
|---|---|---|---|---|
| Google Ads | OAuth2+DevToken | 高 | 中 | あり(GAQL) |
| Meta Ads | OAuth2+BM | 高 | 中 | あり |
| Yahoo!広告 | OAuth2 | 中 | 低 | あり |
| LINE Ads | OAuth2 | 中 | 低 | あり |
| X Ads | OAuth2 | 中 | 高 | 制限 |
| TikTok Ads | OAuth2 | 中 | 中 | あり |

**確定すべき判断 `[手動]`:** 着手1〜2PF(規約リスク低+日本市場有利なら Yahoo!/LINE 軸)、スコープ(構造診断・判断ログ・クリエイティブ評価=作る / 自動入稿・自動運用・自動生成=作らない)。

**受け入れ:** [ ] api-research.md 完成 [ ] 着手PF確定 [ ] スコープ確定 [ ] 規約リスク評価済み

**Cursorプロンプト:**

```
各広告PFの公式APIドキュメントを調査し docs/api-research.md にまとめる(認証・主要読取エンドポイント・
レート制限・自動化に関する規約条項)。着手PF・スコープ・規約判断は私が決めるので材料を提示。法的判断はしない。
```

---

### N4 — mos-kit 抽出(共通基盤・読み取り専用基底)

**目的:** 3編の重複をゼロにする共有パッケージ。**広告/SNS着手の前提。** 自動入稿/投稿をここで構造排除。
**依存:** N1(SEO core が安定APIであること)
**成果物:** `@start-x-work/mos-kit` v0.1.0、SEO編が依存切替(v1.1.0)

**リポ判断:** 専用リポ `start-x-work/mos-kit` 新設 + npm公開(各編が疎結合に依存)。

**実装核(最重要 — 読み取り専用基底):**

```typescript
// mos-kit/src/platform-base.ts
// 外部プラットフォーム連携は読み取り専用が基本。
// 書き込み(入稿/投稿/予約/削除)メソッドはこの基底に存在しない = 構造的に自動運用不可。
export interface ReadOnlyPlatform<TEntity, TMetrics> {
  list(): Promise<TEntity[]>;
  get(id: string): Promise<TEntity>;
  getMetrics(id: string): Promise<TMetrics>;
  // create/update/post/publish/schedule/delete は定義しない
}

// mos-kit/src/ai/provider.ts
export interface AIProvider {
  complete(prompt: string, opts?: CompleteOptions): Promise<string>;
  embed(texts: string[]): Promise<number[][]>;
}
export function createProvider(model?: "gemini"|"openai"|"anthropic", apiKey?: string): AIProvider;

// mos-kit/src/errors.ts — CliError/FetchError/AIError
// mos-kit/src/http/fetch-page.ts — cheerio込み、15sタイムアウト
// mos-kit/src/output/ — json/table/markdown レンダラ
```

**mos-kit 構成:**

```
mos-kit/src/{ ai/, errors.ts, http/, output/, platform-base.ts, index.ts }
```

**SEO編の依存切替(N4の一部):**

```bash
cd ~/projects/marketing-os-seo
pnpm add @start-x-work/mos-kit
# core/src/{ai,errors,http} を削除 → mos-kit import に置換、全テスト緑、v1.1.0再公開
```

**受け入れ:** [ ] mos-kit公開 [ ] platform-base に書込メソッド無 [ ] SEO編が依存切替・テスト緑 [ ] SEO編 v1.1.0

**Cursorプロンプト:**

```
@implementation_blueprint_v2.md N4 に従い、SEO編の ai/errors/http/output を新リポ mos-kit に抽出。
platform-base.ts(ReadOnlyPlatform、書込メソッド無)を新規。抽出後 SEO編を mos-kit 依存に切替え全テスト緑。
npm公開は私。
```

---

### N5 — 広告 CLI(mos-kit利用)

**目的:** `marketing-os-ads` v0.1。診断・評価・構造化のみ。自動運用なし。
**依存:** N4(mos-kit)、N3(スコープ確定)
**成果物:** `@start-x-work/mos-ads` v0.1.0

**実装核(書き込み構造排除 + 機能):**

```typescript
// platforms/provider.ts — 基底継承で書込不可
import type { ReadOnlyPlatform } from "@start-x-work/mos-kit";
export interface Campaign { id:string; name:string; status:string; budget:number }
export interface CampaignMetrics { campaignId:string; impressions:number; clicks:number; cost:number; conversions:number }
export type AdPlatform = ReadOnlyPlatform<Campaign, CampaignMetrics>;

// campaign/decision-log.ts — 人間の判断を構造化(自動実行しない)
import { z } from "zod";
export const DecisionSchema = z.object({
  id:z.string(), timestamp:z.string(), campaignId:z.string(),
  decision:z.string(), rationale:z.string(), expectedOutcome:z.string(),
  metricsSnapshot:z.record(z.number()).optional(),
});

// campaign/structure.ts — 構造診断(SEOのサイト診断と同思想)
export interface CampaignStructure { campaign:string; adGroups:{name:string;keywords:string[];ads:string[]}[]; issues:string[] }

// creative/evaluate.ts — 評価のみ(generate無)
export const CreativeEvalSchema = z.object({
  creative:z.string(),
  scores:z.object({ clarity:z.number(), relevance:z.number(), cta:z.number(), compliance:z.number() }),
  feedback:z.array(z.string()),
});
```

**CLI:** `mos-ads campaign analyze` / `mos-ads campaign log` / `mos-ads creative evaluate "<text>"`

**受け入れ:** [ ] mos-kit依存(コピー無) [ ] 書込API不在 [ ] generate無 [ ] v0.1.0公開

**Cursorプロンプト:**

```
@phase6_ads_impl_v1.md に従い marketing-os-ads v0.1 実装。基盤は @start-x-work/mos-kit 依存(コピー禁止)。
AdPlatform は ReadOnlyPlatform 継承(書込不可)。creative は evaluate のみ generate 作らない。
着手PFは api-research.md の確定値。公開は私。
```

---

### N6 — SNS CLI(mos-kit利用)

**目的:** `marketing-os-social` v0.1。投稿評価・カレンダー診断・アカウント診断。自動投稿なし。
**依存:** N4(mos-kit)。N5とは独立 → **N5と並列可**
**成果物:** `@start-x-work/mos-social` v0.1.0

**実装核(Content OS知見をコード化):**

```typescript
// platforms/provider.ts
import type { ReadOnlyPlatform } from "@start-x-work/mos-kit";
export type SocialPlatform = ReadOnlyPlatform<Post, Engagement> & {
  getProfile(handle: string): Promise<Profile>;
}; // post/publish/schedule 構造的に不可

// post/url-placement.ts — 本文直接URLのアルゴリズムペナルティ(Content OS知見)
export function checkUrlPlacement(text: string, platform: string) {
  const urls = text.match(/https?:\/\/[^\s]+/g) ?? [];
  const penalty = ["x","threads","instagram"].includes(platform) && urls.length > 0;
  return {
    score: penalty ? 40 : 100,
    warnings: penalty ? [`本文直接URL(${urls.length}件)。${platform}でリーチ低下。プロフィール/返信欄へ。`] : [],
  };
}

// calendar/promo-ratio.ts — 1/10ルール(Content OS知見)
export function checkPromoRatio(posts: {type:string}[]) {
  const ratio = posts.filter(p=>p.type==="promotional").length / Math.max(posts.length,1);
  return { ratio, warnings: ratio>0.1 ? [`販促比率${(ratio*100).toFixed(0)}%。1/10ルール超過。`] : [] };
}

// platforms/manual.ts — API制約PF用、手動エクスポート(CSV/JSON)読込フォールバック
```

**CLI:** `mos-social post evaluate "<text>" --platform x` / `mos-social calendar analyze plan.yaml` / `mos-social account audit <handle>`

**API現実対応:** v0.1はAPI非依存の評価機能(post/calendar)が中心。API連携は読取可能なもの限定 + manual.ts フォールバック。

**受け入れ:** [ ] mos-kit依存 [ ] 投稿系API不在 [ ] URL配置・1/10診断動作 [ ] v0.1.0公開 [ ] **3カテゴリ完成**

**Cursorプロンプト:**

```
@phase7_social_impl_v1.md に従い marketing-os-social v0.1 実装。基盤は mos-kit 依存。
SocialPlatform は ReadOnlyPlatform 継承(投稿系不可)。post は evaluate のみ。
Content OS知見必須:url-placement(本文直接URL警告)、promo-ratio(1/10ルール)。
API非依存の評価機能を中心、manual.ts でCSV/JSONフォールバック。公開は私。
```

---

### N7 — 広告 Web UI / N8 — SNS Web UI

**目的:** N2(SEO Web)の構成を広告・SNSへ横展開。
**依存:** N7←N5、N8←N6。互いに並列可。
**成果物:** 広告Web / SNS Web(Cloudflare Pages)

**実装核:** N2 の構成(Workers API + React/Vite + 共通デザイントークン)を各編 `packages/web` にコピー。各編 core を Workers から呼ぶ。

| 編 | Web機能 |
|---|---|
| 広告(N7) | campaign analyze / creative evaluate |
| SNS(N8) | post evaluate / calendar analyze |

**共通UI抽象(任意・発展):** 3編Web完成後、共通コンポーネント(ScoreCard/CheckList/Layout)を `@start-x-work/mos-kit-web` に抽出検討。**早すぎる抽象化を避け、3編揃ってから。**

**受け入れ:** [ ] 各編Web動作 [ ] OS Indigo統一 [ ] 各編core再利用(書換無)

**Cursorプロンプト(N7/N8共通):**

```
@implementation_blueprint_v2.md N7/N8 に従い、SEO編Web構成(N2)を広告/SNS編 packages/web に展開。
Workers API + React/Vite/Tailwind、デザイントークン共通(OS Indigo)。各編 core 再利用、書換禁止。
```

---

### N9 — 統合CLI(marketing-os)

**目的:** 3編を1エントリで束ねる。`marketing-os <category> <command>`。
**依存:** SEO CLI(N0)+ N5 + N6
**成果物:** `@start-x-work/marketing-os`、統合リポ `start-x-work/marketing-os`

**実装核:**

```typescript
// 各編CLIが command エクスポートを公開(統合から取込可能に)
// marketing-os/src/index.ts
import { defineCommand, runMain } from "citty";
const main = defineCommand({
  meta: { name: "marketing-os", description: "Marketing-OS unified CLI" },
  subCommands: {
    seo: () => import("@start-x-work/mos-seo/command").then(m=>m.default),
    ads: () => import("@start-x-work/mos-ads/command").then(m=>m.default),
    social: () => import("@start-x-work/mos-social/command").then(m=>m.default),
  },
});
runMain(main);
```

**思想:** 束ねるだけ。各編は単体でも動く疎結合を維持。統合で自動運用は生まれない。

**受け入れ:** [ ] `npx @start-x-work/marketing-os seo audit ...` 動作 [ ] 3編呼べる [ ] 各編単体動作維持

**Cursorプロンプト:**

```
@implementation_blueprint_v2.md N9 に従い、各編CLIに command エクスポート追加、
marketing-os 統合リポで3編を citty subCommands で束ねる。疎結合維持。公開は私。
```

---

### N10 — 商用導線(随時・全並列)

**目的:** OSS→商用Marketing-OSへの控えめな導線。
**依存:** なし(どのノードとも並列)
**成果物:** 各編CLI出力末尾・Webフッター・READMEの導線

**実装核:** 共通文言を mos-kit に1箇所定義。

```typescript
// mos-kit/src/promo.ts
export const COMMERCIAL_HINT =
  "継続運用・チームでの意思決定には Marketing-OS → https://marketing-os.jp";
// CLI出力末尾に1行、Webフッターに1リンク。煽りなし、1/10精神。
```

**受け入れ:** [ ] 各編に導線 [ ] 煽り表現なし [ ] 思想を損なわない

---

## 4. 横断不変条件(全ノード共通)

| 項目 | 規律 |
|---|---|
| 思想 | 診断・評価・構造化まで。自動生成/実行/投稿/入稿/最適化はしない |
| 構造保証 | mos-kit `ReadOnlyPlatform` に書込メソッドを定義しない |
| 生成排除 | creative/post は evaluate のみ、generate を作らない |
| 技術 | TS strict / pnpm / citty / tsup / vitest / Biome / zod、AIは抽象層経由、mos-kit依存(コピー禁止) |
| ブランド | Apache 2.0 / OS Indigo #5957EE + Slate #0A2540 / NOTICE導線 / 攻撃的語彙禁止 |
| 認証情報 | ユーザー手動設定。Cursor/Claudeは扱わない。.env非コミット、.env.exampleのみ |
| データDB | 巨大キーワードDBを持たない(GSC実データ+推定) |
| GitHub掲載 | 事業数値(MRR等)を載せない。OSS指標のみ |
| 稼働 | Part 9 週20h。並列は待ち時間ゼロ化であって稼働増ではない |

---

## 5. 完成判定(全ノード)

| ノード | 完了条件 |
|---|---|
| N0 | SEO CLI npm公開 + Release |
| N1 | SEO v1.0(安定API + v0.2機能) |
| N2 | SEO Web 公開 |
| N3 | 広告調査・着手PF・スコープ確定 |
| N4 | mos-kit公開 + SEO編依存切替 |
| N5 | 広告CLI公開(mos-kit利用・書込無) |
| N6 | SNS CLI公開(3カテゴリ完成) |
| N7 | 広告Web公開 |
| N8 | SNS Web公開 |
| N9 | 統合CLI公開 |
| N10 | 各編に商用導線 |

**構想完成 =** N0〜N9 完了 + N10 反映。3カテゴリ × (CLI+Web) × mos-kit共通化 × 統合CLI × 商用導線、すべて境界を構造的に保持。

---

## 6. 最速実行プロトコル(まとめ)

1. **今すぐ N0** を実行(SEO CLI公開)。
2. N0完了と同時に **N1・N2・N3 を並列起動**(N3はコード不要なので即・先行消化)。
3. N1完了で **N4(mos-kit)**。これがクリティカルパスの要。
4. N4完了で **N5・N6 を並列**。N3確定済みなので広告に待ちなし。
5. 各CLI完了で **N7・N8 を並列**。
6. 3編CLI揃ったら **N9(統合)**。
7. **N10 は最初から最後までいつでも**差し込む。

クリティカルパス `N0→N1→N4→N5→N7→N9` だけが直列。残りは全部その横で並列に倒す。これが最速。

---

## 7. Cursor 一括起動用インデックス

| ノード | 参照詳細 | 即着手可否 |
|---|---|---|
| N0 | phase3_seo_cli_finalize | ○ 即 |
| N1 | phase5(系統A) | N0後 |
| N2 | phase4_seo_web | N0後・N1と並列 |
| N3 | phase5(系統B) | ○ 即(並列推奨) |
| N4 | [remaining nodes Part A](./remaining_nodes_completion_v1.md) | N1後 |
| N5 | phase6(コピー→mos-kit依存に読替) | N4後 |
| N6 | phase7(コピー→mos-kit依存に読替) | N4後・N5と並列 |
| N7/N8 | [remaining nodes Part B](./remaining_nodes_completion_v1.md) + phase4パターン | 各CLI後 |
| N9 | [remaining nodes Part C](./remaining_nodes_completion_v1.md) | 3編CLI後 |
| N10 | [remaining nodes Part D](./remaining_nodes_completion_v1.md) | 随時 |

---

## 8. バージョン

- v2.0: 2026-06-07。ロードマップ(時系列)を排し、依存グラフ + 並列化で最速完成を定義。全ノードに型・スキーマ・受け入れ条件・検証・プロンプトを付与。`implementation_master_v1.md` を置換。

---

**末尾:**

最速とは焦って同時に手を広げることではなく、依存のない作業を待たせないこと。クリティカルパス `N0→N1→N4→N5→N7→N9` を一直線に通し、その横で N2・N3・N10 を並走させる。順序(依存)と境界(診断・評価まで)だけは崩さない。そこを守る限り、速さは品質を犠牲にしない。

依存順に。並列に倒す。境界は越えない。
