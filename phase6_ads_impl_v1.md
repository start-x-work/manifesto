# Phase 6 — 広告編 v0.1 実装指示書 v1.0

**作成日:** 2026年6月7日
**対象リポジトリ:** `start-x-work/marketing-os-ads`
**前提:** Phase 5(SEO v1.0 + 広告編準備)完了。着手プラットフォーム・スコープ確定済み
**実行環境:** Cursor + ローカル `pnpm` / `npm` / `gh`
**ゴール:** 広告編 v0.1 CLI を実装・公開する(SEO編の基盤を再利用)

---

## 0. このドキュメントについて

### 0-1. 設計の核

**SEO編(`marketing-os-seo`)の構成をコピーして始める。** AI抽象層・monorepo・出力整形・エラー型・CI・配布の仕組みは、SEO編で検証済みのものをそのまま流用する。広告編固有なのは「広告プラットフォーム連携」と「広告特有の評価ロジック」だけ。

### 0-2. スコープの大原則

Phase 5 の規約リスク評価で確定した通り、広告編は**「診断・評価・構造化」にとどめ、自動運用・自動入稿はしない**。これは:
- Marketing-OS思想(意思決定支援、実行代替しない)
- プラットフォーム規約リスクの回避
- 競合(自動運用SaaS)との差別化

の3つを同時に満たす。

### 0-3. 着手スコープ(Phase 5 B4 で確定したもの)

本書では以下を前提とする(Phase 5で確定した内容に置き換えること):

| 機能 | v0.1 |
|---|---|
| キャンペーン設計・配信判断のログ構造化 | ★ 実装 |
| クリエイティブ評価の再現可能な手続き | ★ 実装 |
| 広告プラットフォームデータ取得(読み取りのみ) | △ 限定実装 |
| 自動運用・自動入稿・自動生成 | ✗ 実装しない |

着手プラットフォームは Phase 5 B2 で確定したもの(想定: Yahoo!広告 / LINE Ads など規約リスクが低く日本市場で有利なもの。確定値に置き換える)。

---

## 1. ゴールと完了条件

```
Phase 6 完了条件:
- [ ] marketing-os-ads が monorepo として初期化済み(SEO構成流用)
- [ ] 広告プラットフォームAPI抽象層が実装済み(読み取り中心)
- [ ] キャンペーン設計・配信判断のログ構造化機能が動作
- [ ] クリエイティブ評価機能が動作
- [ ] CLI(mos-ads)から実行できる
- [ ] v0.1.0 が npm 公開済み・Release作成済み
- [ ] 広告編Manifestoと実装が一致
```

---

## 2. 確定技術スタック

SEO編と**完全に同一**。

| 層 | 技術 |
|---|---|
| ランタイム | Node.js 22 LTS |
| 言語 | TypeScript 5.x(strict) |
| パッケージ管理 | pnpm workspace |
| CLIフレームワーク | citty |
| ビルド | tsup |
| テスト | vitest |
| Lint/Format | Biome |
| スキーマ検証 | zod |
| AI | 抽象層経由(SEO編の `ai/provider.ts` を流用) |
| 配布 | npm: `@start-x-work/mos-ads` |
| ライセンス | Apache 2.0 |

---

## 3. モノレポ構成

SEO編の構成を踏襲。

```
marketing-os-ads/
├── package.json
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── biome.json
├── .env.example
├── packages/
│   ├── core/
│   │   ├── src/
│   │   │   ├── ai/              # SEO編からコピー(provider.ts/gemini.ts)
│   │   │   ├── platforms/      # 広告プラットフォーム抽象層 ★広告固有
│   │   │   │   ├── provider.ts # AdPlatform interface
│   │   │   │   ├── yahoo.ts    # (確定プラットフォーム)
│   │   │   │   └── line.ts
│   │   │   ├── campaign/       # キャンペーン設計ログ ★広告固有
│   │   │   │   ├── structure.ts
│   │   │   │   └── decision-log.ts
│   │   │   ├── creative/       # クリエイティブ評価 ★広告固有
│   │   │   │   └── evaluate.ts
│   │   │   ├── errors.ts        # SEO編からコピー
│   │   │   ├── types.ts
│   │   │   └── index.ts
│   │   └── package.json
│   ├── cli/
│   │   ├── src/
│   │   │   ├── commands/
│   │   │   │   ├── campaign.ts  # mos-ads campaign ...
│   │   │   │   └── creative.ts  # mos-ads creative ...
│   │   │   ├── output/          # SEO編からコピー
│   │   │   └── index.ts
│   │   └── package.json         # bin: mos-ads
│   └── web/                     # 将来のWeb UI(今は空)
└── docs/
    ├── ROADMAP.md
    ├── api-research.md          # Phase 5 B1 の成果物
    └── architecture.md          # Phase 5 B6 の成果物
```

---

## 4. タスク分解 + 実装スケルトン

### P6-T1: monorepo初期化(SEO構成流用)`[Cursor/CLI]`

SEO編の設定ファイルをコピーし、広告編用に調整。

```bash
cd ~/projects/marketing-os-ads

# SEO編から設定をコピー(手動 or Cursorで)
# pnpm-workspace.yaml, tsconfig.base.json, biome.json, .env.example

# root package.json
cat > package.json <<'EOF'
{
  "name": "@start-x-work/marketing-os-ads",
  "version": "0.0.0",
  "private": true,
  "license": "Apache-2.0",
  "author": "Start-X LLC <https://marketing-os.jp>",
  "scripts": {
    "build": "pnpm -r build",
    "test": "vitest run",
    "lint": "biome check .",
    "format": "biome format --write ."
  },
  "devDependencies": {
    "@biomejs/biome": "^1.9.0",
    "typescript": "^5.6.0",
    "tsup": "^8.3.0",
    "vitest": "^2.1.0"
  },
  "packageManager": "pnpm@9.0.0"
}
EOF

mkdir -p packages/core/src/{ai,platforms,campaign,creative} packages/cli/src/{commands,output} packages/web docs
```

**Cursorへの指示:** SEO編(`marketing-os-seo`)の `ai/`・`errors.ts`・`output/` をこのリポにコピーせよ。これらは検証済みなので変更しない。

**受け入れ条件:** `pnpm install` が通る。AI抽象層・エラー型・出力整形がSEO編と同一。

### P6-T2: 広告プラットフォームAPI抽象層 `[Cursor]`

AI抽象層と同じ思想で、広告プラットフォームを抽象化。**読み取り中心**(書き込み=自動入稿はしない)。

```typescript
// packages/core/src/platforms/provider.ts
export interface Campaign {
  id: string;
  name: string;
  status: string;
  budget: number;
  // 読み取り用の正規化された構造
}

export interface AdPlatform {
  /** キャンペーン一覧を取得(読み取りのみ) */
  listCampaigns(): Promise<Campaign[]>;
  /** キャンペーンの実績データを取得(読み取りのみ) */
  getMetrics(campaignId: string): Promise<CampaignMetrics>;
  // 書き込み系メソッドは定義しない(自動入稿しない)
}

export interface CampaignMetrics {
  campaignId: string;
  impressions: number;
  clicks: number;
  cost: number;
  conversions: number;
}
```

```typescript
// packages/core/src/platforms/yahoo.ts(確定プラットフォームに置き換え)
import type { AdPlatform, Campaign, CampaignMetrics } from "./provider";

export class YahooAdsPlatform implements AdPlatform {
  constructor(private token = process.env.YAHOO_ADS_TOKEN) {}
  async listCampaigns(): Promise<Campaign[]> {
    // Yahoo!広告API(読み取り)
    return [];
  }
  async getMetrics(campaignId: string): Promise<CampaignMetrics> {
    // 実績取得(読み取り)
    return { campaignId, impressions: 0, clicks: 0, cost: 0, conversions: 0 };
  }
}
```

**重要:** interface に書き込み系メソッド(createCampaign / updateBudget 等)を**定義しない**。読み取り専用にすることで、自動運用を構造的に排除する。

**受け入れ条件:** プラットフォームからキャンペーン一覧・実績を**読み取れる**。書き込みAPIは存在しない。

### P6-T3: キャンペーン設計・配信判断のログ構造化 `[Cursor]`

広告運用の「意思決定」を構造化して記録する。自動実行ではなく、人間の判断を構造化する。

```typescript
// packages/core/src/campaign/decision-log.ts
import { z } from "zod";

export const DecisionSchema = z.object({
  id: z.string(),
  timestamp: z.string(),
  campaignId: z.string(),
  decision: z.string(),        // 何を決めたか
  rationale: z.string(),       // なぜそう判断したか
  expectedOutcome: z.string(), // 期待する結果
  metricsSnapshot: z.record(z.number()).optional(), // 判断時点の実績
});
export type Decision = z.infer<typeof DecisionSchema>;

// 配信判断を構造化して記録(人間の判断を支援、自動実行しない)
export function recordDecision(input: Omit<Decision, "id" | "timestamp">): Decision {
  return {
    id: crypto.randomUUID(),
    timestamp: new Date().toISOString(),
    ...input,
  };
}
```

```typescript
// packages/core/src/campaign/structure.ts
// キャンペーン構造を整理・可視化(設計支援)
export interface CampaignStructure {
  campaign: string;
  adGroups: { name: string; keywords: string[]; ads: string[] }[];
  issues: string[]; // 構造上の問題点(診断)
}

export function analyzeStructure(campaign: Campaign): CampaignStructure {
  // 構造を分析し、問題点を診断(SEOのサイト診断と同じ思想)
  return { campaign: campaign.name, adGroups: [], issues: [] };
}
```

**思想:** SEO編の「サイト診断」と同じ。広告キャンペーンの構造を**診断**し、意思決定を**記録**する。実行はしない。

**受け入れ条件:** `mos-ads campaign analyze` で構造診断、`mos-ads campaign log` で判断記録ができる。

### P6-T4: クリエイティブ評価 `[Cursor]`

クリエイティブを**生成せず、評価する**。再現可能な評価手続きを提供。

```typescript
// packages/core/src/creative/evaluate.ts
import type { AIProvider } from "../ai";
import { z } from "zod";

export const CreativeEvalSchema = z.object({
  creative: z.string(),
  scores: z.object({
    clarity: z.number(),       // 明確さ
    relevance: z.number(),     // 関連性
    cta: z.number(),           // CTAの強さ
    compliance: z.number(),    // 規約適合性
  }),
  feedback: z.array(z.string()),
});
export type CreativeEval = z.infer<typeof CreativeEvalSchema>;

// クリエイティブを評価(生成ではない)
export async function evaluateCreative(
  ai: AIProvider,
  creative: string,
): Promise<CreativeEval> {
  const prompt = `次の広告クリエイティブを、明確さ・関連性・CTA・規約適合性で評価しJSON出力。生成や書き換えはしない: "${creative}"`;
  const json = await ai.complete(prompt, { json: true });
  return CreativeEvalSchema.parse(JSON.parse(json));
}
```

**思想:** 評価であって生成ではない。AdCreative等の「自動生成」競合とは明確に異なる。既存クリエイティブの質を診断する。

**受け入れ条件:** `mos-ads creative evaluate "<text>"` で評価スコア + フィードバックが返る。生成機能はない。

### P6-T5: CLI統合・公開 `[Cursor]` + `[CLI]`

```typescript
// packages/cli/src/index.ts
import { defineCommand, runMain } from "citty";

const main = defineCommand({
  meta: { name: "mos-ads", description: "Marketing-OS Ads toolkit" },
  subCommands: {
    campaign: () => import("./commands/campaign").then((m) => m.default),
    creative: () => import("./commands/creative").then((m) => m.default),
  },
});
runMain(main);
```

公開:

```bash
cd ~/projects/marketing-os-ads
pnpm lint && pnpm build && pnpm test
cd packages/cli && npm publish   # @start-x-work/mos-ads
cd ~/projects/marketing-os-ads
git tag v0.1.0 && git push origin v0.1.0
gh release create v0.1.0 --repo start-x-work/marketing-os-ads \
  --title "Ads Toolkit v0.1.0 (CLI)" \
  --notes "Campaign structure analysis, decision logging, creative evaluation. Diagnosis and evaluation only — no automated operation."
```

**受け入れ条件:** `npx @start-x-work/mos-ads campaign analyze` が動作。Release作成済み。

### P6-T6: 公開アナウンス `[手動]`

広告編リリースを発信。SEO編に次ぐ2カテゴリ目の公開。「自動運用ツールではなく、意思決定を構造化する道具」という差別化を明確に伝える。

---

## 5. Definition of Done

- [ ] monorepo初期化(SEO構成流用)(P6-T1)
- [ ] 広告プラットフォーム抽象層(読み取り専用)(P6-T2)
- [ ] キャンペーン構造診断・判断ログ(P6-T3)
- [ ] クリエイティブ評価(P6-T4)
- [ ] CLI統合・v0.1.0公開(P6-T5)
- [ ] アナウンス(P6-T6、手動)
- [ ] 書き込み系API(自動入稿)を一切実装していない

すべて満たせば **広告編 v0.1 完了 → SNS編(将来)へ**。

---

## 6. やらないこと(構造的に排除)

| やらない | 排除方法 |
|---|---|
| 自動入稿・自動運用 | AdPlatform interface に書き込みメソッドを定義しない |
| クリエイティブ自動生成 | evaluate のみ実装、generate は作らない |
| 予算自動調整 | 判断ログ(人間の判断記録)のみ |
| 規約リスクの高い操作 | 読み取り専用に限定 |

---

## 7. Cursor Agent への指示(コピペ用)

```
@phase6_ads_impl_v1.md に従い、marketing-os-ads の v0.1 を実装します。

P6-T1 から。SEO編(marketing-os-seo)の ai/・errors.ts・output/ をコピーして
基盤を作り、広告編固有の platforms/・campaign/・creative/ を実装します。

必須ルール(構造的制約):
- AdPlatform interface に書き込み系メソッドを定義しない(読み取り専用)
- クリエイティブは evaluate のみ。generate は作らない
- 自動運用・自動入稿・予算自動調整は実装しない
- AI は SEO編からコピーした provider 経由
- 着手プラットフォームは Phase 5 で確定したもの(api-research.md 参照)

P6-T5 の npm公開・P6-T6 のアナウンスは私が確認してから。
各タスクの受け入れ条件を満たしてから次へ。各段階で commit。
規約に関わる判断は私が行います。
```

---

## 8. 以降(将来)

- SNS編 v0.1(3カテゴリの最後、`marketing-os-social`)
- 3カテゴリを束ねる `marketing-os` メタブランド検討
- 各編の Web UI 展開

---

## 9. バージョン管理

- v1.0: 2026年6月7日 初版
