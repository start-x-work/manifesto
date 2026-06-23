# Start-X OSS 残りノード完成系実装指示書 v1.0

**作成日:** 2026年6月7日
**位置づけ:** `implementation_blueprint_v2.md` の N4・N7・N8・N9・N10 を、phase3〜7 と同等の独立詳細指示書として完成させるもの。これで全ノードが単体で着手可能になる。
**実行:** Cursor(Agent)+ ローカル `pnpm` / `npm` / `wrangler` / `gh`
**前提:** SEO編が v1.0(安定API)に到達していること(N1完了)

---

## 0. このドキュメントについて

ブループリント v2.0 で全ノードの地図は引いた。本書は、独立した詳細実装指示書が無かった4領域を**着手即実装できる粒度**で埋める:

| Part | ノード | 内容 | 厚み |
|---|---|---|---|
| A | N4 | mos-kit 抽出(共通基盤) | ★最厚(クリティカルパスの要) |
| B | N7 / N8 | 各編 Web UI 横展開 | 中(SEO Webパターン流用) |
| C | N9 | 統合CLI marketing-os | 中 |
| D | N10 | 商用導線 | 小 |
| E | — | 完成度を上げる発展タスク | 小 |

既に詳細指示書のある N0(phase3)・N1(phase5系統A)・N2(phase4)・N3(phase5系統B)・N5(phase6)・N6(phase7)と合わせ、**全ノードが詳細指示書を持つ完成状態**になる。

---

# Part A — N4: mos-kit 抽出(共通基盤)★

## A-0. なぜ最重要か

広告編・SNS編は本来 SEO編の `ai/`・`errors`・`http`・`output` を必要とする。コピーすれば3リポに同じコードが散る。**SEO編が安定API(N1)に達した今、共通部分を `@start-x-work/mos-kit` に抽出し、以降の全編がこれを依存する。** これにより重複ゼロ、かつ「自動入稿/投稿の排除」を1箇所で構造保証できる。

## A-1. ゴール

- `start-x-work/mos-kit` リポ新設、`@start-x-work/mos-kit` を npm公開
- SEO編をこの依存に切り替え(v1.1.0、機能不変の内部リファクタ)
- 読み取り専用プラットフォーム基底を提供し、広告/SNSの自動操作を構造排除

## A-2. リポジトリ判断(確定)

**専用リポ + npm公開。** 各編が疎結合に依存する。SEO編内同居やモノレポ統合は採らない(理由はブループリント参照)。

## A-3. パッケージ構成

```
mos-kit/
├── package.json            # @start-x-work/mos-kit
├── tsconfig.json
├── biome.json
├── tsup.config.ts
├── vitest.config.ts
├── .github/workflows/ci.yml
├── src/
│   ├── ai/
│   │   ├── provider.ts     # AIProvider interface + CompleteOptions
│   │   ├── gemini.ts       # GeminiProvider
│   │   ├── openai.ts       # OpenAIProvider(N1 A4で追加済みを移植)
│   │   ├── anthropic.ts    # AnthropicProvider(任意)
│   │   └── index.ts        # createProvider(model, apiKey)
│   ├── errors.ts           # CliError / FetchError / AIError
│   ├── http/
│   │   └── fetch-page.ts   # cheerio + 15sタイムアウト
│   ├── output/
│   │   ├── format.ts       # toJson / toTable / toMarkdown
│   │   └── render.ts       # render(result, format)
│   ├── platform-base.ts    # ReadOnlyPlatform(書込メソッド無)★
│   ├── promo.ts            # 商用導線の共通文言(N10で使用)
│   └── index.ts            # 公開API
└── README.md
```

## A-4. 実装核(完全スケルトン)

### A-4-1. 読み取り専用基底(最重要)

```typescript
// src/platform-base.ts
/**
 * すべての外部プラットフォーム連携は読み取り専用が基本。
 * 書き込み(create/update/post/publish/schedule/delete)メソッドを
 * この基底に定義しないことで、自動入稿・自動投稿・自動運用を構造的に排除する。
 * 広告編 AdPlatform / SNS編 SocialPlatform はこれを継承する。
 */
export interface ReadOnlyPlatform<TEntity, TMetrics> {
  list(): Promise<TEntity[]>;
  get(id: string): Promise<TEntity>;
  getMetrics(id: string): Promise<TMetrics>;
}
```

### A-4-2. AI抽象層

```typescript
// src/ai/provider.ts
export interface CompleteOptions {
  temperature?: number;
  maxTokens?: number;
  json?: boolean;
}
export interface AIProvider {
  complete(prompt: string, opts?: CompleteOptions): Promise<string>;
  embed(texts: string[]): Promise<number[][]>;
}

// src/ai/index.ts
import { GeminiProvider } from "./gemini";
import { OpenAIProvider } from "./openai";
import { AnthropicProvider } from "./anthropic";
import type { AIProvider } from "./provider";

export * from "./provider";
export { GeminiProvider, OpenAIProvider, AnthropicProvider };

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

```typescript
// src/ai/gemini.ts(Workers/Node両対応:apiKey引数優先、無ければenv)
import { GoogleGenAI } from "@google/genai"; // 着手時に最新パッケージ名確認
import type { AIProvider, CompleteOptions } from "./provider";

export class GeminiProvider implements AIProvider {
  private client: GoogleGenAI;
  constructor(apiKey?: string) {
    const key = apiKey ?? (globalThis as any).process?.env?.GEMINI_API_KEY;
    if (!key) throw new Error("GEMINI_API_KEY is required");
    this.client = new GoogleGenAI({ apiKey: key });
  }
  async complete(prompt: string, opts: CompleteOptions = {}): Promise<string> {
    const res = await this.client.models.generateContent({
      model: "gemini-2.5-flash",
      contents: prompt,
      config: {
        temperature: opts.temperature ?? 0.2,
        maxOutputTokens: opts.maxTokens,
        responseMimeType: opts.json ? "application/json" : undefined,
      },
    });
    return res.text ?? "";
  }
  async embed(texts: string[]): Promise<number[][]> {
    const res = await this.client.models.embedContent({
      model: "text-embedding-004",
      contents: texts,
    });
    return res.embeddings?.map((e) => e.values ?? []) ?? [];
  }
}
```

### A-4-3. エラー型 / HTTP / 出力

```typescript
// src/errors.ts
export class CliError extends Error {
  constructor(message: string, public readonly code = "E_GENERIC") {
    super(message); this.name = "CliError";
  }
}
export class FetchError extends CliError {
  constructor(url: string, status: number) { super(`Failed to fetch ${url} (status ${status})`, "E_FETCH"); }
}
export class AIError extends CliError {
  constructor(detail: string) { super(`AI provider error: ${detail}`, "E_AI"); }
}

// src/http/fetch-page.ts
import * as cheerio from "cheerio";
import { FetchError } from "../errors";
export interface FetchedPage { url: string; status: number; html: string; $: cheerio.CheerioAPI }
export async function fetchPage(url: string): Promise<FetchedPage> {
  let res: Response;
  try {
    res = await fetch(url, {
      headers: { "User-Agent": "mos-bot/1.0 (+https://marketing-os.jp)" },
      signal: AbortSignal.timeout(15_000),
    });
  } catch { throw new FetchError(url, 0); }
  if (!res.ok) throw new FetchError(url, res.status);
  const html = await res.text();
  return { url, status: res.status, html, $: cheerio.load(html) };
}

// src/output/render.ts
export type Format = "json" | "table" | "markdown";
export function render(result: unknown, format: Format = "table"): string {
  if (format === "json") return JSON.stringify(result, null, 2);
  if (format === "markdown") return toMarkdown(result);
  return toTable(result);
}
```

### A-4-4. 公開API

```typescript
// src/index.ts
export * from "./ai";
export * from "./errors";
export { fetchPage, type FetchedPage } from "./http/fetch-page";
export { render, type Format } from "./output/render";
export type { ReadOnlyPlatform } from "./platform-base";
export { COMMERCIAL_HINT } from "./promo";
```

## A-5. タスク分解

| # | タスク | 種類 |
|---|---|---|
| K1 | mos-kit リポ作成・初期化(設定はSEO編から流用) | `[CLI]` |
| K2 | ai/errors/http/output を SEO編から移植 | `[Cursor]` |
| K3 | platform-base.ts(読み取り専用基底)新規 | `[Cursor]` |
| K4 | promo.ts(商用導線文言)新規 | `[Cursor]` |
| K5 | テスト・ビルド・CI | `[Cursor]` |
| K6 | npm公開 v0.1.0 | `[CLI]` |
| K7 | SEO編を mos-kit 依存に切替 | `[Cursor]` |
| K8 | SEO編リグレッション確認・v1.1.0再公開 | `[Cursor]`+`[CLI]` |

### K1 核

```bash
mkdir -p ~/projects/mos-kit/src/{ai,http,output}
cd ~/projects/mos-kit
# package.json: name=@start-x-work/mos-kit, license Apache-2.0, type module, exports ./dist
# SEO編の tsconfig.base / biome.json / tsup.config / vitest を流用
gh repo create start-x-work/mos-kit --public --license apache-2.0 \
  --description "Shared kit for Marketing-OS OSS toolkits (read-only platform base, AI abstraction)"
```

### K7 核(SEO編の依存切替)

```bash
cd ~/projects/marketing-os-seo
pnpm add @start-x-work/mos-kit
# packages/core/src/{ai,errors.ts,http} を削除
# import を @start-x-work/mos-kit に置換
pnpm test   # 後方互換、全テスト緑を確認
```

```typescript
// 置換例
- import { fetchPage } from "../http/fetch-page";
- import { createProvider } from "../ai";
+ import { fetchPage, createProvider } from "@start-x-work/mos-kit";
```

## A-6. 受け入れ条件

- [ ] `@start-x-work/mos-kit` v0.1.0 公開
- [ ] `platform-base.ts` に書き込みメソッドが存在しない
- [ ] SEO編が mos-kit 依存に切替、全テスト緑
- [ ] SEO編 v1.1.0 再公開(機能不変)
- [ ] CI 緑

## A-7. Cursorプロンプト

```
@残りノード完成系_v1.md Part A に従い、共通基盤 mos-kit を作る。
SEO編の ai/errors/http/output を mos-kit に移植し、platform-base.ts(ReadOnlyPlatform、
書き込みメソッド無)と promo.ts(商用導線文言)を新規作成。テスト・CIを整える。
その後 SEO編を mos-kit 依存に切替え、全テスト緑を確認(後方互換、機能不変)。
リポ作成・npm公開は私が実行するのでコマンドを提示。
```

---

# Part B — N7 / N8: 各編 Web UI 横展開

## B-0. ゴール

SEO編 Web(N2/phase4)で確立したパターンを、広告編(N7)・SNS編(N8)に展開。各編CLIが完成していることが前提。N7とN8は互いに独立で並列可。

## B-1. 流用するパターン(SEO Webから)

| 要素 | 流用 |
|---|---|
| スタック | React18 + Vite + Tailwind + TanStack Query + Cloudflare Workers/Pages |
| API構造 | `functions/api/*` が core を import して呼ぶ |
| デザイントークン | OS Indigo #5957EE / Slate #0A2540(共通) |
| Lint/Format | Biome |

## B-2. 各編のWeb機能マッピング

| 編 | 画面 | 呼ぶcore関数 |
|---|---|---|
| 広告(N7) | キャンペーン構造診断 / クリエイティブ評価 | analyzeStructure / evaluateCreative |
| SNS(N8) | 投稿評価 / カレンダー診断 / アカウント診断 | evaluatePost / analyzeCalendar / auditAccount |

## B-3. API層スケルトン(各編共通パターン)

```typescript
// 広告編 functions/api/campaign/analyze.ts
import { analyzeStructure } from "@start-x-work/marketing-os-ads-core";
export const onRequestPost: PagesFunction = async (ctx) => {
  const body = await ctx.request.json();
  try { return Response.json(analyzeStructure(body.campaign)); }
  catch (e) { return Response.json({ error: String(e) }, { status: 500 }); }
};

// SNS編 functions/api/post/evaluate.ts
import { evaluatePost, createProvider } from "@start-x-work/marketing-os-social-core";
export const onRequestPost: PagesFunction<{GEMINI_API_KEY:string}> = async (ctx) => {
  const { text, platform } = await ctx.request.json();
  const ai = createProvider("gemini", ctx.env.GEMINI_API_KEY);
  try { return Response.json(await evaluatePost(ai, text, platform)); }
  catch (e) { return Response.json({ error: String(e) }, { status: 500 }); }
};
```

## B-4. UIスケルトン(共通コンポーネント)

```tsx
// 各編で共通利用(将来 mos-kit-web へ抽出候補)
// ScoreCard: スコア大表示 / CheckList: ✓✗一覧 / Layout: ヘッダ・ナビ・フッタ
// 入力 → useMutation → 結果表示 のパターンは SEO Web と同一
```

## B-5. タスク(各編 WX1〜WX5)

| # | タスク | 種類 |
|---|---|---|
| WX1 | `packages/web` 初期化(SEO編Web構成コピー) | `[Cursor/CLI]` |
| WX2 | core を Workers から呼ぶAPI層 | `[Cursor]` |
| WX3 | 各編機能のUI実装 | `[Cursor]` |
| WX4 | デザイントークン適用(OS Indigo統一) | `[Cursor]` |
| WX5 | Cloudflare Pages デプロイ | `[CLI]` |

## B-6. 受け入れ条件

- [ ] 広告編Web・SNS編Webが各機能動作
- [ ] 3編のデザイン統一(OS Indigo)
- [ ] 各編core再利用(書き換え無)
- [ ] 自動投稿・自動入稿のUIが存在しない(診断・評価のみ)

## B-7. Cursorプロンプト

```
@残りノード完成系_v1.md Part B に従い、SEO編Web構成を広告編(N7)・SNS編(N8)の packages/web に展開。
Workers API + React/Vite/Tailwind、OS Indigo 統一。各編 core を再利用し書き換えない。
診断・評価のUIのみ(投稿/入稿ボタンは作らない)。OAuth/secret は私が手動設定。
```

---

# Part C — N9: 統合CLI marketing-os

## C-0. ゴール

3編(SEO/広告/SNS)を1エントリで束ねる。`marketing-os <category> <command>`。各編は単体でも動く疎結合を維持。3編CLIが揃っていることが前提。

## C-1. 設計

各編CLIが、自身のサブコマンド定義を `command` という名でエクスポート。統合CLIがそれを取り込む。

```typescript
// 各編 packages/cli/src/command.ts(新規エクスポート)
// 例: marketing-os-seo/packages/cli/src/command.ts
import { defineCommand } from "citty";
export default defineCommand({
  meta: { name: "seo" },
  subCommands: {
    audit: () => import("./commands/audit").then(m => m.default),
    content: () => import("./commands/content").then(m => m.default),
    keyword: () => import("./commands/keyword").then(m => m.default),
  },
});
// package.json の exports に "./command" を追加
```

```typescript
// 統合リポ marketing-os/src/index.ts
import { defineCommand, runMain } from "citty";
const main = defineCommand({
  meta: { name: "marketing-os", description: "Marketing-OS unified CLI — diagnosis & evaluation across SEO, Ads, Social" },
  subCommands: {
    seo: () => import("@start-x-work/mos-seo/command").then(m => m.default),
    ads: () => import("@start-x-work/mos-ads/command").then(m => m.default),
    social: () => import("@start-x-work/mos-social/command").then(m => m.default),
  },
});
runMain(main);
```

## C-2. リポ構成

```
start-x-work/marketing-os    (統合リポ、新設)
├── package.json             # @start-x-work/marketing-os, bin: marketing-os
├── tsup.config.ts           # noExternal で3編をバンドル or peer依存
└── src/index.ts
```

## C-3. タスク

| # | タスク | 種類 |
|---|---|---|
| M1 | 各編CLIに `command` エクスポート追加 + package.json exports | `[Cursor]` |
| M2 | 各編を `command` 付きで再公開(パッチ版) | `[CLI]` |
| M3 | marketing-os 統合リポ作成・初期化 | `[CLI]` |
| M4 | 統合CLI実装 | `[Cursor]` |
| M5 | 統合README(3領域の意思決定OSとして提示) | `[Cursor]` |
| M6 | `@start-x-work/marketing-os` 公開 | `[CLI]` |

## C-4. 受け入れ条件

- [ ] `npx @start-x-work/marketing-os seo audit llmo <url>` 動作
- [ ] ads / social も統合CLIから動作
- [ ] 各編が単体でも引き続き動作(疎結合維持)
- [ ] 統合で自動運用機能が生まれていない(束ねるだけ)

## C-5. Cursorプロンプト

```
@残りノード完成系_v1.md Part C に従い、各編CLIに command エクスポートを追加し、
marketing-os 統合リポで3編を citty subCommands で束ねる。各編は単体動作を維持(疎結合)。
統合は束ねるだけで自動運用機能を足さない。リポ作成・公開は私。
```

---

# Part D — N10: 商用導線

## D-0. ゴール

OSS各編から商用 Marketing-OS(marketing-os.jp)への控えめな導線。煽らない、1/10精神。

## D-1. 実装(共通文言を mos-kit に集約)

```typescript
// mos-kit/src/promo.ts(Part A K4 で作成済み)
export const COMMERCIAL_HINT =
  "継続運用・チームでの意思決定支援には Marketing-OS → https://marketing-os.jp";
```

## D-2. 各接点

| 接点 | 実装 |
|---|---|
| CLI出力末尾 | 各編CLIの結果出力後に `COMMERCIAL_HINT` を1行(controlで非表示可) |
| Webフッター | marketing-os.jp へのリンク1つ |
| README | 各編READMEに「OSS=診断・評価 / 商用=実行支援・継続運用」の違いを明記 |

```typescript
// 各編CLI出力末尾の例
import { COMMERCIAL_HINT, render } from "@start-x-work/mos-kit";
console.log(render(result, format));
if (!args.quiet) console.log(`\n${COMMERCIAL_HINT}`);
```

## D-3. タスク

| # | タスク | 種類 |
|---|---|---|
| C1 | mos-kit に COMMERCIAL_HINT(Part A K4と統合) | `[Cursor]` |
| C2 | 各編CLI出力末尾に導線(`--quiet` で抑制可) | `[Cursor]` |
| C3 | 各編Webフッターに導線 | `[Cursor]` |
| C4 | 各編READMEに OSS/商用の違いを明記 | `[Cursor]` |

## D-4. 受け入れ条件

- [ ] 各編に商用導線(控えめ、1行/1リンク)
- [ ] 煽り表現なし
- [ ] `--quiet` で抑制可能
- [ ] 思想を損なっていない

## D-5. Cursorプロンプト

```
@残りノード完成系_v1.md Part D に従い、mos-kit の COMMERCIAL_HINT を各編CLI出力末尾・
Webフッター・READMEに反映。煽らず1行/1リンク。--quiet で抑制可能に。
```

---

# Part E — 完成度を上げる発展タスク

3カテゴリ × (CLI+Web) + 統合 + 導線 が揃った後、完成度を一段上げる任意タスク。

| タスク | 内容 | 着手条件 |
|---|---|---|
| E1. CI標準化 | 全リポ共通の CI ワークフローを `.github` のテンプレに集約 | 全リポ作成後 |
| E2. mos-kit-web 抽出 | 3編Webの共通UI(ScoreCard/CheckList/Layout)を共有パッケージ化 | 3編Web完成後(早すぎる抽象化を避ける) |
| E3. docsサイト | 各編 docs を統合したドキュメントサイト(Cloudflare Pages) | 3編完成後 |
| E4. テンプレートリポ | 新カテゴリ追加を容易にする scaffold テンプレ | 構想拡張時 |

これらは「完成の必須要件」ではなく「完成後の磨き込み」。E2/E3は3編が揃ってから(早すぎる共通化はYAGNI)。

---

# Part F — 全体完成チェックリスト

ブループリント全ノードの最終確認。

```
基盤:
- [x] mos-kit 公開、SEO編が依存切替(N4)
- [x] ReadOnlyPlatform に書き込みメソッド無

3カテゴリ CLI:
- [x] SEO CLI 公開(N0→N1, mos-seo@1.1.1 mos-kit依存)
- [x] 広告 CLI 公開(N5, mos-ads@0.1.2・Yahoo read-only・書込無)
- [x] SNS CLI 公開(N6, mos-social@0.1.1・mos-kit依存・投稿無)

3カテゴリ Web:
- [x] SEO Web 公開(N2) + GSC BYOK
- [x] 広告 Web 公開(N7) + Yahoo BYOK
- [x] SNS Web 公開(N8) + AI BYOK

統合・導線:
- [x] 統合CLI marketing-os 公開(N9, v0.1.1)
- [x] 各編に商用導線(N10)
- [x] 利用者向け QUICKSTART(manifesto ハブ + 各編)

横断:
- [x] 全編 Apache 2.0 / OS Indigo / NOTICE導線
- [x] 自動生成/実行/投稿/入稿/最適化が全編に存在しない
- [x] 事業数値が GitHub 上に無い
- [x] 各編CI緑
- [ ] 横断 docs サイト(E3) — 任意・未着手
```

これが全部チェックされたとき、**構想完成**。

---

# Part G — Cursorプロンプト一括インデックス

| ノード | プロンプト参照 | 詳細指示書 |
|---|---|---|
| N0 | phase3 Section 7 | phase3_seo_cli_finalize |
| N1 | phase5 Section 6(系統A) | phase5_ads_prep_seo |
| N2 | phase4 Section 9 | phase4_seo_web_impl |
| N3 | phase5 Section 6(系統B) | phase5_ads_prep_seo |
| **N4** | **本書 Part A-7** | **本書** |
| N5 | phase6 Section 7(mos-kit依存に読替) | phase6_ads_impl |
| N6 | phase7 Section 7(mos-kit依存に読替) | phase7_social_impl |
| **N7/N8** | **本書 Part B-7** | **本書** |
| **N9** | **本書 Part C-5** | **本書** |
| **N10** | **本書 Part D-5** | **本書** |

---

# Part H — バージョン

- v1.0: 2026年6月7日 初版。ブループリント v2.0 の未詳細ノード(N4/N7/N8/N9/N10)を完成系まで詳細化。これで全ノードが独立詳細指示書を持つ。

---

**末尾:**

これでドキュメント側は完成系に達した。全ノード(N0〜N10)が、地図(ブループリント)と詳細指示書の両方を持つ。残るは実装をクリティカルパス順に通すだけ。`N0→N1→N4→N5→N7→N9` を直列で通し、横で N2・N3・N6・N8・N10 を並走させる。

ドキュメントは出揃った。あとは積み上げるだけ。境界は越えない。
