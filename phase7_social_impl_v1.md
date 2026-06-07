# Phase 7 — SNS編 v0.1 実装指示書 v1.0

**作成日:** 2026年6月7日
**対象リポジトリ:** `start-x-work/marketing-os-social`
**前提:** Phase 6(広告編 v0.1)完了。SEO編・広告編の基盤が確立済み
**実行環境:** Cursor + ローカル `pnpm` / `npm` / `gh`
**ゴール:** SNS編 v0.1 CLI を実装・公開し、3カテゴリ(SEO/広告/SNS)を完成させる

---

## 0. このドキュメントについて

### 0-1. 位置づけ

3カテゴリの**最後**。SEO編・広告編で確立した基盤(AI抽象層・monorepo・読み取り専用プラットフォーム抽象層・出力整形)をそのまま流用する。これが完成すると、Start-X OSS の当初構想(マーケティング3領域)が揃う。

### 0-2. SNS編で最も警戒すべきこと

**自動投稿。** SNS領域はマーケティングツールが最も「自動投稿・自動運用」に流れやすい。Marketing-OS思想(運用マニュアル Part 9: 自動投稿は永続的に禁止)を、広告編と同じく**interface設計で構造的に排除**する。`SocialPlatform` に投稿系メソッド(post / publish / schedule)を定義しない。

### 0-3. SNS編独自の価値

Start-X の Content OS 運用知見をコード化する。これが既存SNSツールとの差別化になる:

| 知見 | コード化 |
|---|---|
| 1/10ルール(販促は全投稿の最大10%) | カレンダー診断で販促比率をチェック |
| LP URL を本文内に直接置くとアルゴリズムペナルティ | 投稿評価でURL配置を診断、プロフィール/返信誘導を提案 |
| プラットフォーム別の最適構造 | 投稿評価をプラットフォーム別に調整 |

### 0-4. SNS APIの現実

SNS APIは制約が厳しい(X有料化、Instagram審査厳格、Threads/note APIが限定的)。そこで v0.1 は**API非依存の評価機能を中心**に据え、API連携は読み取り可能なものに限定、手動エクスポートデータ(CSV/JSON)の読み込みもサポートする。これにより API 制約に縛られず価値を出せる。

---

## 1. ゴールと完了条件

```
Phase 7 完了条件:
- [ ] marketing-os-social が monorepo として初期化済み(SEO/広告構成流用)
- [ ] 投稿評価機能が動作(API不要、Content OS知見を反映)
- [ ] コンテンツカレンダー診断が動作(1/10ルール等)
- [ ] アカウント/プロフィール診断が動作(読み取り可能な範囲)
- [ ] SNSプラットフォーム抽象層(読み取り専用)が実装済み
- [ ] CLI(mos-social)から実行できる
- [ ] v0.1.0 が npm 公開済み・Release作成済み
- [ ] SNS編Manifestoと実装が一致
- [ ] 投稿系API(自動投稿)を一切実装していない
```

---

## 2. 確定技術スタック

SEO編・広告編と**完全に同一**。

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
| ファイル解析 | 投稿計画の YAML/CSV 読み込み(zod + papaparse 等) |
| AI | 抽象層経由(SEO/広告編の `ai/provider.ts` 流用) |
| 配布 | npm: `@start-x-work/mos-social` |
| ライセンス | Apache 2.0 |

---

## 3. モノレポ構成

```
marketing-os-social/
├── package.json
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── biome.json
├── .env.example
├── packages/
│   ├── core/
│   │   ├── src/
│   │   │   ├── ai/              # SEO/広告編からコピー
│   │   │   ├── platforms/      # SNSプラットフォーム抽象層(読み取り専用)★
│   │   │   │   ├── provider.ts # SocialPlatform interface
│   │   │   │   ├── threads.ts
│   │   │   │   ├── x.ts
│   │   │   │   └── manual.ts   # 手動エクスポートデータ読み込み
│   │   │   ├── post/           # 投稿評価 ★
│   │   │   │   ├── evaluate.ts
│   │   │   │   └── url-placement.ts  # LP URL配置診断(Content OS知見)
│   │   │   ├── calendar/       # コンテンツカレンダー診断 ★
│   │   │   │   ├── analyze.ts
│   │   │   │   └── promo-ratio.ts    # 1/10ルール(Content OS知見)
│   │   │   ├── account/        # アカウント/プロフィール診断 ★
│   │   │   │   └── audit.ts
│   │   │   ├── errors.ts        # SEO/広告編からコピー
│   │   │   ├── types.ts
│   │   │   └── index.ts
│   │   └── package.json
│   ├── cli/
│   │   ├── src/
│   │   │   ├── commands/
│   │   │   │   ├── post.ts       # mos-social post ...
│   │   │   │   ├── calendar.ts   # mos-social calendar ...
│   │   │   │   └── account.ts    # mos-social account ...
│   │   │   ├── output/          # SEO/広告編からコピー
│   │   │   └── index.ts
│   │   └── package.json         # bin: mos-social
│   └── web/                     # 将来のWeb UI(今は空)
└── docs/
    ├── ROADMAP.md
    └── USAGE.md
```

---

## 4. タスク分解 + 実装スケルトン

### P7-T1: monorepo初期化(SEO/広告構成流用)`[Cursor/CLI]`

```bash
cd ~/projects/marketing-os-social

# root package.json
cat > package.json <<'EOF'
{
  "name": "@start-x-work/marketing-os-social",
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

mkdir -p packages/core/src/{ai,platforms,post,calendar,account} packages/cli/src/{commands,output} packages/web docs
```

**Cursorへの指示:** SEO編または広告編から `ai/`・`errors.ts`・`output/` をコピーせよ。検証済みなので変更しない。`pnpm-workspace.yaml`・`tsconfig.base.json`・`biome.json`・`.env.example` も流用。

**受け入れ条件:** `pnpm install` が通る。共通基盤がSEO/広告編と同一。

### P7-T2: SNSプラットフォーム抽象層(読み取り専用)`[Cursor]`

広告編の `AdPlatform` と同じ思想。**読み取りのみ。投稿系メソッドを定義しない。**

```typescript
// packages/core/src/platforms/provider.ts
export interface Profile {
  handle: string;
  platform: string;
  bio: string;
  links: string[];        // プロフィール内のリンク
  followerCount?: number;
}

export interface Post {
  id: string;
  text: string;
  createdAt: string;
  engagement?: Engagement;
}

export interface Engagement {
  likes: number;
  reposts: number;
  replies: number;
  impressions?: number;
}

export interface SocialPlatform {
  /** プロフィールを取得(読み取りのみ) */
  getProfile(handle: string): Promise<Profile>;
  /** 投稿を取得(読み取りのみ) */
  getPosts(handle: string, limit?: number): Promise<Post[]>;
  /** エンゲージメントを取得(読み取りのみ) */
  getEngagement(postId: string): Promise<Engagement>;
  // post / publish / schedule / delete は定義しない(自動投稿しない)
}
```

```typescript
// packages/core/src/platforms/manual.ts
// API が使えないプラットフォーム用。手動エクスポートした CSV/JSON を読み込む。
import type { SocialPlatform, Post, Profile } from "./provider";

export class ManualDataPlatform implements SocialPlatform {
  constructor(private dataPath: string) {}
  async getProfile(handle: string): Promise<Profile> {
    // エクスポートファイルから読み込み
    return { handle, platform: "manual", bio: "", links: [] };
  }
  async getPosts(): Promise<Post[]> {
    // CSV/JSON をパース
    return [];
  }
  async getEngagement(): Promise<Engagement> {
    return { likes: 0, reposts: 0, replies: 0 };
  }
}
```

**重要:** interface に書き込み系を定義しないことで、自動投稿を構造的に排除。`manual.ts` により API 制約のあるプラットフォームでも価値を出せる。

**受け入れ条件:** プラットフォームからプロフィール・投稿・エンゲージメントを**読み取れる**。手動データ読み込みが動作。投稿系APIは存在しない。

### P7-T3: 投稿評価(Content OS知見を反映)`[Cursor]`

投稿テキストを評価する。**生成しない。** API不要で動く中心機能。

```typescript
// packages/core/src/post/evaluate.ts
import type { AIProvider } from "../ai";
import { z } from "zod";
import { checkUrlPlacement } from "./url-placement";

export const PostEvalSchema = z.object({
  text: z.string(),
  platform: z.string(),
  scores: z.object({
    hook: z.number(),          // 冒頭の引き
    clarity: z.number(),       // 明確さ
    engagement: z.number(),    // エンゲージメント誘発度
    urlPlacement: z.number(),  // URL配置の適切さ(Content OS知見)
  }),
  warnings: z.array(z.string()),
  feedback: z.array(z.string()),
});
export type PostEval = z.infer<typeof PostEvalSchema>;

export async function evaluatePost(
  ai: AIProvider,
  text: string,
  platform = "x",
): Promise<PostEval> {
  // URL配置診断(Content OS知見、ローカルロジック)
  const urlCheck = checkUrlPlacement(text, platform);

  // AI評価(生成ではなく評価)
  const prompt = `次の${platform}投稿を hook/clarity/engagement で評価しJSON出力。書き換えや生成はしない: "${text}"`;
  const json = await ai.complete(prompt, { json: true });
  const aiEval = JSON.parse(json);

  return PostEvalSchema.parse({
    text,
    platform,
    scores: { ...aiEval.scores, urlPlacement: urlCheck.score },
    warnings: [...urlCheck.warnings],
    feedback: aiEval.feedback ?? [],
  });
}
```

```typescript
// packages/core/src/post/url-placement.ts
// Content OS知見: 本文内に直接 LP URL を置くとアルゴリズムペナルティ
export interface UrlPlacementCheck {
  score: number;       // 0-100
  warnings: string[];
}

export function checkUrlPlacement(text: string, platform: string): UrlPlacementCheck {
  const urlRegex = /https?:\/\/[^\s]+/g;
  const urls = text.match(urlRegex) ?? [];
  const warnings: string[] = [];
  let score = 100;

  // X / Threads など、本文URLがリーチを下げるプラットフォーム
  const penaltyPlatforms = ["x", "threads", "instagram"];
  if (penaltyPlatforms.includes(platform) && urls.length > 0) {
    score = 40;
    warnings.push(
      `本文に直接URLがあります(${urls.length}件)。${platform}ではリーチ低下の可能性。プロフィールリンクや返信欄への配置を検討してください。`,
    );
  }
  return { score, warnings };
}
```

**思想:** 評価であって生成ではない。Content OS の運用知見(URL配置)をコード化し、既存ツールにない診断を提供。

**受け入れ条件:** `mos-social post evaluate "<text>" --platform x` でスコア + 警告 + フィードバックが返る。本文直接URLに警告が出る。生成機能はない。

### P7-T4: コンテンツカレンダー診断(1/10ルール)`[Cursor]`

投稿計画ファイル(YAML/CSV)を読み込み、構造を診断する。

```typescript
// packages/core/src/calendar/analyze.ts
import { z } from "zod";
import { checkPromoRatio } from "./promo-ratio";

export const PlannedPostSchema = z.object({
  date: z.string(),
  platform: z.string(),
  text: z.string(),
  type: z.enum(["value", "promotional", "engagement"]).default("value"),
});
export type PlannedPost = z.infer<typeof PlannedPostSchema>;

export interface CalendarAnalysis {
  totalPosts: number;
  byPlatform: Record<string, number>;
  promoRatio: number;
  cadenceIssues: string[];
  warnings: string[];
}

export function analyzeCalendar(posts: PlannedPost[]): CalendarAnalysis {
  const promo = checkPromoRatio(posts); // 1/10ルール
  const byPlatform: Record<string, number> = {};
  for (const p of posts) byPlatform[p.platform] = (byPlatform[p.platform] ?? 0) + 1;

  return {
    totalPosts: posts.length,
    byPlatform,
    promoRatio: promo.ratio,
    cadenceIssues: [], // 投稿頻度の一貫性チェック
    warnings: promo.warnings,
  };
}
```

```typescript
// packages/core/src/calendar/promo-ratio.ts
// Content OS知見: 販促コンテンツは全投稿の最大10%
export interface PromoRatioCheck {
  ratio: number;
  warnings: string[];
}

export function checkPromoRatio(posts: { type: string }[]): PromoRatioCheck {
  if (posts.length === 0) return { ratio: 0, warnings: [] };
  const promoCount = posts.filter((p) => p.type === "promotional").length;
  const ratio = promoCount / posts.length;
  const warnings: string[] = [];
  if (ratio > 0.1) {
    warnings.push(
      `販促投稿の比率が ${(ratio * 100).toFixed(0)}% です。1/10ルール(10%以下)を超えています。価値提供型の投稿を増やすことを検討してください。`,
    );
  }
  return { ratio, warnings };
}
```

**受け入れ条件:** `mos-social calendar analyze plan.yaml` で、販促比率・プラットフォーム別投稿数・警告が返る。1/10ルール超過で警告。

### P7-T5: アカウント/プロフィール診断 `[Cursor]`

プロフィールがプラットフォームで最適化されているか診断(SEOのサイト診断、広告の構造診断に相当)。

```typescript
// packages/core/src/account/audit.ts
import type { SocialPlatform, Profile } from "../platforms/provider";

export interface AccountAudit {
  handle: string;
  platform: string;
  checks: { id: string; label: string; passed: boolean; detail: string }[];
  recommendations: string[];
}

export async function auditAccount(
  platform: SocialPlatform,
  handle: string,
): Promise<AccountAudit> {
  const profile = await platform.getProfile(handle);
  const checks = [
    checkBio(profile),
    checkProfileLink(profile),   // Content OS知見: LP URLはプロフィールに置く
    // ...
  ];
  return {
    handle,
    platform: profile.platform,
    checks,
    recommendations: checks.filter((c) => !c.passed).map((c) => c.detail),
  };
}

function checkProfileLink(p: Profile) {
  return {
    id: "profile-link",
    label: "プロフィールにリンクがあるか",
    passed: p.links.length > 0,
    detail: p.links.length === 0
      ? "プロフィールにリンクがありません。本文URLの代わりにプロフィールリンクを活用してください。"
      : "プロフィールリンクが設定されています。",
  };
}

function checkBio(p: Profile) {
  return {
    id: "bio",
    label: "bioが設定されているか",
    passed: p.bio.length > 0,
    detail: p.bio.length === 0 ? "bioが空です。" : "bioが設定されています。",
  };
}
```

**受け入れ条件:** `mos-social account audit <handle> --platform x` でプロフィール診断が返る。

### P7-T6: CLI統合・公開 `[Cursor]` + `[CLI]`

```typescript
// packages/cli/src/index.ts
import { defineCommand, runMain } from "citty";

const main = defineCommand({
  meta: { name: "mos-social", description: "Marketing-OS Social toolkit" },
  subCommands: {
    post: () => import("./commands/post").then((m) => m.default),
    calendar: () => import("./commands/calendar").then((m) => m.default),
    account: () => import("./commands/account").then((m) => m.default),
  },
});
runMain(main);
```

公開:

```bash
cd ~/projects/marketing-os-social
pnpm lint && pnpm build && pnpm test
cd packages/cli && npm publish   # @start-x-work/mos-social
cd ~/projects/marketing-os-social
git tag v0.1.0 && git push origin v0.1.0
gh release create v0.1.0 --repo start-x-work/marketing-os-social \
  --title "Social Toolkit v0.1.0 (CLI)" \
  --notes "Post evaluation, content calendar analysis (1/10 rule), account audit. Diagnosis and evaluation only — no automated posting."
```

**受け入れ条件:** `npx @start-x-work/mos-social post evaluate "..."` が動作。Release作成済み。

### P7-T7: SNS編Manifesto更新 + 公開アナウンス `[Cursor]` + `[手動]`

- `manifesto/social/README.md` を「Coming Soon」から実装内容へ更新(日英併記)
- 3カテゴリ完成のアナウンス。「自動投稿ツールではなく、SNS運用の意思決定を構造化する道具」+「3領域が揃った」ことを発信

**受け入れ条件:** SNS編Manifestoが実装と一致。アナウンス完了(手動)。

---

## 5. Definition of Done

- [ ] monorepo初期化(SEO/広告構成流用)(P7-T1)
- [ ] SNSプラットフォーム抽象層(読み取り専用)(P7-T2)
- [ ] 投稿評価(URL配置診断含む)(P7-T3)
- [ ] カレンダー診断(1/10ルール)(P7-T4)
- [ ] アカウント診断(P7-T5)
- [ ] CLI統合・v0.1.0公開(P7-T6)
- [ ] Manifesto更新・アナウンス(P7-T7)
- [ ] 投稿系API(自動投稿)を一切実装していない

すべて満たせば **SNS編 v0.1 完了 → 3カテゴリ(SEO/広告/SNS)完成**。

---

## 6. やらないこと(構造的に排除)

| やらない | 排除方法 |
|---|---|
| 自動投稿・予約投稿 | SocialPlatform に post/publish/schedule を定義しない |
| コンテンツ自動生成 | evaluate のみ実装、generate は作らない |
| エンゲージメント自動化(自動いいね等) | 読み取り専用、書き込みなし |
| フォロワー自動獲得 | 実装しない(規約違反・思想違反) |

---

## 7. Cursor Agent への指示(コピペ用)

```
@phase7_social_impl_v1.md に従い、marketing-os-social の v0.1 を実装します。

P7-T1 から。SEO編または広告編の ai/・errors.ts・output/ をコピーして基盤を作り、
SNS編固有の platforms/・post/・calendar/・account/ を実装します。

必須ルール(構造的制約):
- SocialPlatform interface に post/publish/schedule/delete を定義しない(読み取り専用)
- 投稿は evaluate のみ。generate は作らない
- 自動投稿・予約投稿・自動いいね・フォロワー自動獲得は実装しない
- AI はコピーした provider 経由

Content OS知見を必ず反映:
- post: 本文内の直接URLを診断(X/Threads/Instagramでペナルティ警告、プロフィール/返信誘導を提案)
- calendar: 1/10ルール(販促投稿は全体の10%以下)をチェック

API制約に注意:
- v0.1 は API非依存の評価機能(post evaluate / calendar analyze)を中心に
- API連携は読み取り可能なものに限定
- platforms/manual.ts で手動エクスポートデータ(CSV/JSON)読み込みに対応

P7-T6 の npm公開・P7-T7 のアナウンスは私が確認してから。
各タスクの受け入れ条件を満たしてから次へ。各段階で commit。
```

---

## 8. 以降(3カテゴリ完成後)

3カテゴリ(SEO/広告/SNS)が揃った後の検討事項:

| 検討項目 | 内容 |
|---|---|
| メタブランド | 3カテゴリを束ねる `marketing-os` 統合CLI/構想 |
| Web UI展開 | 広告編・SNS編にもWeb UIを(SEO編Phase 4の横展開) |
| 共通coreの抽出 | 3編で重複する基盤(ai/errors/output)を共有パッケージ化 |
| Marketing-OS本体統合 | OSS 3編と商用版の接続深化 |

これらは別途、状況を見て指示書化する。

---

## 9. ロードマップ上の位置づけ

`master_roadmap_v3.md` の「将来: SNS編 → 3カテゴリ完成 → メタブランド検討」のうち、本書は **SNS編** を担当。完了後、master_roadmap を v4 に更新し、3カテゴリ完成を反映する(別途)。

---

## 10. バージョン管理

- v1.0: 2026年6月7日 初版
