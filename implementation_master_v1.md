# Start-X OSS 網羅的実装マスター v1.0

**作成日:** 2026年6月7日
**位置づけ:** 全実装を依存順に1本で進めるための決定版マスター指示書
**対象:** `start-x-work` Organization 全リポジトリ
**実行環境:** Cursor(Agent / Composer)+ ローカル `pnpm` / `npm` / `wrangler` / `gh`
**制約:** 運用マニュアル Part 9(週20時間上限)

---

## 0. このドキュメントについて

### 0-1. これは何か

Phase 3〜7 の個別指示書、および発展タスク(共通core抽出・各編Web展開・メタブランド統合・商用連携)を、**実装の依存順に1本へ統合**したマスター指示書。上から順に実行すれば、SEO編の公開から3カテゴリ完成、その先の統合まで、手が止まらずに進む。

### 0-2. 既存の個別指示書との関係

各ステージの詳細は個別指示書に既にある。本書はそれらを束ね、**依存順・着手順を確定**し、個別指示書に無い発展タスクを新規に詳細化する。

| 個別指示書 | 本書での扱い |
|---|---|
| `phase3_seo_cli_finalize_v1.md` | ステージ1の詳細リファレンス |
| `phase4_seo_web_impl_v1.md` | ステージ2の詳細リファレンス |
| `phase5_ads_prep_seo_v1.md` | ステージ3の詳細リファレンス |
| `phase6_ads_impl_v1.md` | ステージ5の詳細リファレンス |
| `phase7_social_impl_v1.md` | ステージ6の詳細リファレンス |
| (なし) | ステージ4・7・8・9 を本書で新規詳細化 |

### 0-3. 設計上の改善点(個別指示書からの進化)

**共通core(mos-kit)抽出を、広告編着手の直前に挟む。** 個別指示書では Phase 6/7 を「SEO編からコピー」としていたが、コピーは重複を生む。本書では SEO編が安定した時点で共有パッケージ `@start-x-work/mos-kit` を抽出し、広告編・SNS編は最初からそれを使う。これにより3編のコード重複をゼロにする。

---

## 1. 現在地(GitHub事実ベース・2026年6月7日)

| リポジトリ | コミット | Release | npm | 状態 |
|---|---|---|---|---|
| `manifesto` | 完成 | — | — | ✓ 公開済み |
| `marketing-os-seo` | 3 | なし | 未公開 | ◐ コード実装済み・未公開 |
| `marketing-os-ads` | scaffold | — | — | ▷ Coming表記 |
| `marketing-os-social` | scaffold | — | — | ▷ Coming表記 |

**起点:** SEO編はコードがあるが公開されていない。ここから全実装を前進させる。

---

## 2. 実装順序(依存グラフ)

```
ステージ1  Phase 3クローズ(SEO CLI 公開)         ← 今すぐ
   │
ステージ2  Phase 4 SEO Web UI                      ← core再利用
   │
ステージ3  Phase 5 SEO v1.0 + 広告準備             ← coreを安定APIに
   │
ステージ4  共通core抽出(@start-x-work/mos-kit)★   ← 広告編着手の直前
   │
ステージ5  Phase 6 広告 v0.1(mos-kit利用)          ← コピーしない
   │
ステージ6  Phase 7 SNS v0.1(mos-kit利用)
   │
ステージ7  各編Web UI横展開(広告・SNS)★
   │
ステージ8  メタブランド統合(marketing-os 統合CLI)★
   │
ステージ9  OSS→商用連携 ★(随時・並行可)
```

★ = 本書で新規詳細化。並行可能な箇所は各ステージに明記。

### 並行実行マップ(週20h制約内)

| 同時に進められる | 競合しない理由 |
|---|---|
| ステージ2(Web UI実装)+ ステージ3系統B(広告調査) | 実装 vs 調査 |
| ステージ7(各編Web)+ ステージ9(商用連携設計) | UI実装 vs 設計 |

「並行可能」は同時着手できるという意味で、稼働時間を増やす意味ではない。

---

## 3. ステージ1 — Phase 3クローズ(SEO CLI公開)

**詳細:** `phase3_seo_cli_finalize_v1.md`

### ゴール

コードがある `marketing-os-seo` を npm公開 + v0.1.0 Release し、客観的に「公開済み」にする。

### 着手前チェック

コミットが3つのまま=ローカルの仕上げ(テスト/CI/ドキュメント)が未pushの可能性。まず現状をpushしてから公開へ。

### タスク(F1〜F7のうち未了分)

1. ローカル変更を push(未pushなら)
2. テスト・lint・ビルドが緑か確認
3. 入力検証・エラーハンドリングの確認(zod / CliError / タイムアウト)
4. CI(GitHub Actions)を追加
5. npm公開
6. v0.1.0 Release

### 実行コマンド(クローズの核)

```bash
cd ~/projects/marketing-os-seo
git status && git push origin main          # 未pushなら

pnpm install --frozen-lockfile
pnpm lint && pnpm build && pnpm test         # 緑確認

# coreをcliにバンドルして1パッケージ配布(tsup: noExternal)
cd packages/cli && npm publish               # @start-x-work/mos-seo

cd ~/projects/marketing-os-seo
git tag v0.1.0 && git push origin v0.1.0
gh release create v0.1.0 --repo start-x-work/marketing-os-seo \
  --title "SEO Toolkit v0.1.0 (CLI)" \
  --notes "First public release. LLMO/AEO audit, technical SEO audit, content brief, keyword intent mapper. CLI only."

npx @start-x-work/mos-seo audit site https://example.com --format json   # 外部確認
```

### 受け入れ条件(Gate B)

- [ ] `npx @start-x-work/mos-seo` が外部で動作
- [ ] v0.1.0 Release が存在
- [ ] CI 緑

---

## 4. ステージ2 — Phase 4 SEO Web UI

**詳細:** `phase4_seo_web_impl_v1.md`

### ゴール

CLIの4機能を Web UI(`packages/web`)から使えるようにし、Cloudflareで公開。

### 核となる設計

- `packages/core` を書き換えず再利用
- Cloudflare Workers(API)+ React/Vite(UI)
- Google OAuth(GSC連携)
- Stripe DESIGN準拠(OS Indigo #5957EE)

### タスク(W1〜W7)

W1 初期化 → W2 Workers API層 → W3 Google OAuth → W4 4画面UI → W5 デザイン適用 → W6 デプロイ → W7 アナウンス

### core調整(後方互換)

Workersは `process.env` 非対応。`createProvider(apiKey?)` をキー引数対応に(CLIは引数なしで従来通り動く後方互換)。

### 受け入れ条件(Gate C)

- [ ] 4機能がWebで動作
- [ ] GSC OAuth連携が動作
- [ ] Cloudflare Pagesで公開

---

## 5. ステージ3 — Phase 5 SEO v1.0 + 広告準備

**詳細:** `phase5_ads_prep_seo_v1.md`

### 系統A: SEO v1.0

- A1 公開API確定(core を semver 安定APIに)
- A2 キーワードボリューム推定(巨大DBは作らない、GSC実データ+推定)
- A3 多言語対応(`--lang`)
- A4 LLMOマルチモデル対応(抽象層にProvider追加)
- A5 Marketing-OS連携導線
- A6 v1.0.0 リリース

### 系統B: 広告準備(ステージ2と並行可)

- B1 広告API調査(6プラットフォーム評価)
- B2 着手プラットフォーム確定 `[手動]`
- B3 規約リスク評価 `[手動]`
- B4 スコープ確定 `[手動]`
- B5 広告編Manifesto更新
- B6 再利用設計

### 受け入れ条件(Gate D)

- [ ] SEO v1.0.0 公開
- [ ] core が安定API(以降破壊的変更なし)
- [ ] 着手プラットフォーム・スコープ確定

---

## 6. ステージ4 — 共通core抽出(@start-x-work/mos-kit)★新規

### 6-1. なぜここで抽出するか

SEO編の core が ステージ3(A1)で安定APIになった。**広告編に着手する前に**、3編で共通する基盤を共有パッケージ `mos-kit` に切り出す。これにより広告編・SNS編は「コピー」ではなく「依存」で基盤を得る。重複ゼロ。

タイミングの根拠: 早すぎる抽出(SEO編のみの段階)はYAGNIだが、SEO編が安定し2編目に入る直前は、抽出の最適点。

### 6-2. 抽出する共通要素

| 要素 | 元(SEO編) | mos-kit での位置 |
|---|---|---|
| AI抽象層 | `core/src/ai/` | `mos-kit/ai/` |
| エラー型 | `core/src/errors.ts` | `mos-kit/errors.ts` |
| 出力整形 | `cli/src/output/` | `mos-kit/output/` |
| HTTP/fetch | `core/src/http/` | `mos-kit/http/` |
| プラットフォーム抽象パターン | (広告/SNSで使う) | `mos-kit/platform-base.ts` |

### 6-3. リポジトリ構成の判断

**判断: 専用リポ `start-x-work/mos-kit` を新設し、npm公開する。**

| 選択肢 | 評価 |
|---|---|
| A. 専用リポ + npm公開 | ★採用。各編が `@start-x-work/mos-kit` を依存。疎結合 |
| B. SEO編リポ内に同居 | 広告/SNSがSEO編に依存することになり不自然 |
| C. モノレポに3編統合 | 大規模再編。リスク高、今はしない |

### 6-4. mos-kit パッケージ構成

```
mos-kit/
├── package.json          # @start-x-work/mos-kit
├── tsconfig.json
├── biome.json
├── tsup.config.ts
├── src/
│   ├── ai/
│   │   ├── provider.ts   # AIProvider interface
│   │   ├── gemini.ts
│   │   ├── openai.ts     # ステージ3 A4 で追加済みのものを移植
│   │   └── index.ts      # createProvider(model, apiKey)
│   ├── errors.ts         # CliError / FetchError / AIError
│   ├── http/
│   │   └── fetch-page.ts # cheerio込み(SEO/SNSで共用)
│   ├── output/
│   │   ├── format.ts     # json/table/markdown
│   │   └── render.ts
│   ├── platform-base.ts  # 読み取り専用プラットフォーム抽象の基底
│   └── index.ts
└── README.md
```

### 6-5. 読み取り専用プラットフォーム基底(重要)

広告編・SNS編の「自動入稿/自動投稿しない」を **mos-kit レベルで構造的に保証**する。基底interfaceに書き込みメソッドを定義しない。

```typescript
// mos-kit/src/platform-base.ts
// すべての外部プラットフォーム連携は読み取り専用が基本。
// 書き込み(入稿/投稿)メソッドはこの基底に存在しない。
export interface ReadOnlyPlatform<TEntity, TMetrics> {
  list(): Promise<TEntity[]>;
  get(id: string): Promise<TEntity>;
  getMetrics(id: string): Promise<TMetrics>;
  // create / update / post / publish / delete は定義しない
}
```

広告編の `AdPlatform`、SNS編の `SocialPlatform` はこれを継承し、書き込みを構造的に排除する。

### 6-6. タスク

| # | タスク | 種類 |
|---|---|---|
| K1 | `mos-kit` リポ作成・初期化 | `[CLI]` |
| K2 | SEO編から共通要素を移植 | `[Cursor]` |
| K3 | `platform-base.ts`(読み取り専用基底)新規 | `[Cursor]` |
| K4 | mos-kit テスト・ビルド | `[Cursor]` |
| K5 | mos-kit を npm公開(v0.1.0) | `[CLI]` |
| K6 | SEO編を mos-kit 依存に切り替え | `[Cursor]` |
| K7 | SEO編リグレッション確認・再リリース(v1.1.0) | `[Cursor]` + `[CLI]` |

### 6-7. K6 の核(SEO編の依存切り替え)

```bash
# SEO編で
cd ~/projects/marketing-os-seo
pnpm add @start-x-work/mos-kit
# core/src/ai, errors, http を削除し、mos-kit から import に置換
# 既存テストが通ることを確認(後方互換)
pnpm test
```

```typescript
// 置換例: marketing-os-seo/packages/core/src/llmo/audit.ts
- import { fetchPage } from "../http/fetch-page";
+ import { fetchPage } from "@start-x-work/mos-kit";
```

### 6-8. 受け入れ条件

- [ ] `@start-x-work/mos-kit` が npm公開
- [ ] `platform-base.ts` に書き込みメソッドが存在しない
- [ ] SEO編が mos-kit 依存に切り替わり、全テスト緑
- [ ] SEO編 v1.1.0 再リリース(機能変更なし、内部リファクタ)

---

## 7. ステージ5 — Phase 6 広告 v0.1(mos-kit利用)

**詳細:** `phase6_ads_impl_v1.md`(ただし「SEO編からコピー」を「mos-kit依存」に読み替え)

### ゴール

`marketing-os-ads` v0.1 を、mos-kit を使って実装・公開。

### 個別指示書からの変更点

- 基盤(ai/errors/output)は**コピーせず `@start-x-work/mos-kit` を依存**
- `AdPlatform` は `mos-kit` の `ReadOnlyPlatform` を継承(書き込み構造排除)

```typescript
// marketing-os-ads/packages/core/src/platforms/provider.ts
import type { ReadOnlyPlatform } from "@start-x-work/mos-kit";

export type AdPlatform = ReadOnlyPlatform<Campaign, CampaignMetrics>;
// listCampaigns/getMetrics は基底から継承、書き込みは構造的に不可能
```

### スコープ(変更なし)

- キャンペーン構造診断・配信判断ログ(P6-T3)
- クリエイティブ評価(P6-T4、generateなし)
- 読み取り専用プラットフォーム連携(P6-T2)

### 受け入れ条件

- [ ] `@start-x-work/mos-ads` 公開
- [ ] mos-kit を依存(コピーなし)
- [ ] 書き込みAPI不在

---

## 8. ステージ6 — Phase 7 SNS v0.1(mos-kit利用)

**詳細:** `phase7_social_impl_v1.md`(同様に「コピー」を「mos-kit依存」に読み替え)

### ゴール

`marketing-os-social` v0.1 を mos-kit を使って実装・公開。

### 個別指示書からの変更点

- 基盤は mos-kit 依存
- `SocialPlatform` は `ReadOnlyPlatform` を継承(投稿系を構造排除)

```typescript
// marketing-os-social/packages/core/src/platforms/provider.ts
import type { ReadOnlyPlatform } from "@start-x-work/mos-kit";

export type SocialPlatform = ReadOnlyPlatform<Post, Engagement> & {
  getProfile(handle: string): Promise<Profile>;
};
// post/publish/schedule は構造的に不可能
```

### Content OS知見(維持)

- `url-placement.ts`: 本文直接URLのペナルティ警告
- `promo-ratio.ts`: 1/10ルール

### 受け入れ条件

- [ ] `@start-x-work/mos-social` 公開
- [ ] mos-kit 依存
- [ ] 投稿系API不在
- [ ] **3カテゴリ(SEO/広告/SNS)完成**

---

## 9. ステージ7 — 各編Web UI横展開 ★新規

### ゴール

SEO編 Phase 4 で確立したWeb UIパターンを、広告編・SNS編にも展開。

### 設計

SEO編 `packages/web` の構成(Cloudflare Workers + React/Vite + Stripe DESIGN)を各編にコピー。各編のcore機能をWeb化。

| 編 | Web機能 |
|---|---|
| 広告 | キャンペーン構造診断・クリエイティブ評価をWeb UIで |
| SNS | 投稿評価・カレンダー診断をWeb UIで |

### タスク(各編共通)

| # | タスク |
|---|---|
| WX1 | `packages/web` 初期化(SEO編Web構成流用) |
| WX2 | core を Workers から呼ぶAPI層 |
| WX3 | 各編機能のUI実装 |
| WX4 | Stripe DESIGN適用(OS Indigo統一) |
| WX5 | Cloudflare Pages デプロイ |

### 共通UIの mos-kit-web 化(任意・発展)

3編のWeb UIで共通するコンポーネント(ScoreCard / CheckList / Layout)を `@start-x-work/mos-kit-web` に切り出す検討。ただし**3編のWeb UIが揃ってから**(早すぎる抽象化を避ける)。

### 受け入れ条件

- [ ] 広告編・SNS編がそれぞれWebで動作
- [ ] 3編のデザインが統一(OS Indigo)

---

## 10. ステージ8 — メタブランド統合(marketing-os 統合CLI)★新規

### ゴール

3編(SEO/広告/SNS)を束ねる統合エントリポイントを提供。利用者が1つのCLI/UIから全領域にアクセスできる。

### 設計判断

**統合CLI `marketing-os` を新設し、各編CLIをサブコマンドとして束ねる。**

```
marketing-os seo audit llmo <url>      → mos-seo に委譲
marketing-os ads campaign analyze       → mos-ads に委譲
marketing-os social post evaluate "..." → mos-social に委譲
```

### リポジトリ構成

```
start-x-work/marketing-os   (統合リポ、新設)
├── package.json            # @start-x-work/marketing-os
└── src/
    └── index.ts            # 各編CLIをサブコマンドに束ねる(citty)
```

```typescript
// marketing-os/src/index.ts
import { defineCommand, runMain } from "citty";

const main = defineCommand({
  meta: { name: "marketing-os", description: "Marketing-OS unified CLI" },
  subCommands: {
    seo: () => import("@start-x-work/mos-seo/command").then((m) => m.default),
    ads: () => import("@start-x-work/mos-ads/command").then((m) => m.default),
    social: () => import("@start-x-work/mos-social/command").then((m) => m.default),
  },
});
runMain(main);
```

各編CLIは、自身のサブコマンド定義を `command` エクスポートとして公開(統合CLIから取り込めるように)。

### タスク

| # | タスク |
|---|---|
| M1 | 各編CLIが `command` エクスポートを公開するよう調整 |
| M2 | `marketing-os` 統合リポ作成 |
| M3 | 統合CLI実装(3編を束ねる) |
| M4 | 統合READMEで「3領域の意思決定OS」を提示 |
| M5 | `@start-x-work/marketing-os` 公開 |

### 思想の確認

統合は「束ねる」だけ。各編の「診断・評価まで、自動実行しない」境界は維持。統合によって自動運用機能が生まれることはない。

### 受け入れ条件

- [ ] `npx @start-x-work/marketing-os seo audit ...` が動作
- [ ] 3編すべてが統合CLIから呼べる
- [ ] 各編は単体でも引き続き動作(疎結合維持)

---

## 11. ステージ9 — OSS→商用連携 ★新規(随時・並行可)

### ゴール

OSS各編から商用Marketing-OS(marketing-os.jp)への自然な導線を実装。押し付けず、思想を保つ。

### 実装内容

| 接点 | 実装 |
|---|---|
| CLI出力末尾 | 診断結果の後に、継続運用・チーム利用なら商用版、と控えめに1行 |
| Web UI フッター | marketing-os.jp への導線 |
| エクスポート連携 | 診断結果をJSON出力し、商用版が取り込める形式に(将来) |
| README | 各編READMEに商用版との違い(OSS=診断、商用=実行支援+継続)を明記 |

### 重要な制約

- 押し付けない(1/10ルールの精神: 宣伝は最小限)
- OSS利用者を商用に「誘導」するのではなく「選択肢を示す」
- 攻撃的・煽り表現の禁止

### タスク

| # | タスク |
|---|---|
| C1 | 各編CLIの出力末尾に導線(共通文言を mos-kit に) |
| C2 | Web UIフッター導線 |
| C3 | 各編READMEに OSS/商用の違いを明記 |
| C4 | (将来)エクスポート形式の設計 |

### 受け入れ条件

- [ ] 各編に商用導線が存在(控えめ)
- [ ] 思想を損なっていない(煽りなし)

---

## 12. 横断的設計原則(全ステージ不変)

### 12-1. Marketing-OS思想

診断・評価・構造化まで。自動生成・自動実行・自動投稿・自動入稿・自動最適化はしない。mos-kit の `ReadOnlyPlatform` 基底で構造的に保証。

### 12-2. 技術的一貫性

TypeScript strict / pnpm / citty / tsup / vitest / Biome / zod。AIは抽象層(mos-kit)経由。3編はmos-kitに依存(コピーしない)。

### 12-3. ブランド一貫性

Apache 2.0 / OS Indigo #5957EE + Slate #0A2540 / NOTICE で商用導線 / 攻撃的語彙禁止。

### 12-4. 認証情報

APIキー・OAuthシークレットはユーザーが手動設定。Cursor/Claudeは扱わない。`.env` 非コミット、`.env.example` のみ。

### 12-5. Part 9

週20時間上限(特例25h/最大8週間)。並行は稼働増ではない。神経系低下時は即ペースダウン。

---

## 13. Cursorプロンプト集(ステージ別)

各ステージ着手時、対応プロンプトをAgentに渡す。

### ステージ1(Phase 3クローズ)

```
@phase3_seo_cli_finalize_v1.md の F1〜F5 を確認し、未了分を補完。
その後 F6(npm公開・Release)の手順を私に提示してください(公開は私が実行)。
```

### ステージ2(SEO Web UI)

```
@phase4_seo_web_impl_v1.md に従い packages/web を実装。core は書き換えず、
Workers対応の createProvider 調整のみ後方互換で。OAuth設定は私が手動。
```

### ステージ3(SEO v1.0 + 広告準備)

```
@phase5_ads_prep_seo_v1.md に従い系統A(SEO v1.0)と系統B(広告調査)を並行。
規約の法的判断は私。core の公開APIを A1 で確定してください。
```

### ステージ4(mos-kit抽出)

```
@implementation_master_v1.md ステージ4に従い、SEO編の ai/errors/http/output を
@start-x-work/mos-kit に抽出。platform-base.ts(読み取り専用基底、書き込みメソッドなし)を
新規作成。抽出後、SEO編を mos-kit 依存に切り替え、全テスト緑を確認。
npm公開は私が実行します。
```

### ステージ5(広告 v0.1)

```
@phase6_ads_impl_v1.md に従い marketing-os-ads を実装。ただし基盤はコピーせず
@start-x-work/mos-kit を依存。AdPlatform は ReadOnlyPlatform を継承(書き込み不可)。
```

### ステージ6(SNS v0.1)

```
@phase7_social_impl_v1.md に従い marketing-os-social を実装。基盤は mos-kit 依存。
SocialPlatform は ReadOnlyPlatform 継承。Content OS知見(URL配置・1/10ルール)を反映。
```

### ステージ7(各編Web)

```
@implementation_master_v1.md ステージ7に従い、SEO編Web構成を広告編・SNS編に展開。
OS Indigo でデザイン統一。
```

### ステージ8(メタブランド)

```
@implementation_master_v1.md ステージ8に従い、各編CLIに command エクスポートを追加し、
marketing-os 統合CLIで3編を束ねる。各編は単体でも動く疎結合を維持。
```

### ステージ9(商用連携)

```
@implementation_master_v1.md ステージ9に従い、各編に控えめな商用導線を追加。
共通文言は mos-kit に。煽り表現は使わない。
```

---

## 14. Gate / 完了条件サマリー

| ステージ | 完了条件 | 依存 |
|---|---|---|
| 1 | SEO CLI npm公開 + Release | — |
| 2 | SEO Web UI 公開 | 1 |
| 3 | SEO v1.0公開 + 広告スコープ確定 | 2 |
| 4 | mos-kit公開 + SEO編が依存切替 | 3 |
| 5 | 広告 v0.1公開(mos-kit利用) | 4 |
| 6 | SNS v0.1公開(3カテゴリ完成) | 4 |
| 7 | 広告・SNSのWeb UI公開 | 5, 6 |
| 8 | 統合CLI marketing-os 公開 | 5, 6 |
| 9 | 各編に商用導線 | 随時 |

**最終ゴール:** 3カテゴリがCLI+Webで揃い、統合CLIで束ねられ、商用への導線を持ち、すべてが「診断・評価まで」の境界を守っている状態。

---

## 15. やらないこと(全ステージ共通・構造的排除)

| やらない | 排除方法 |
|---|---|
| 自動入稿・自動投稿・自動運用 | mos-kit `ReadOnlyPlatform` に書き込みメソッドを定義しない |
| コンテンツ/クリエイティブ自動生成 | evaluate のみ、generate を作らない |
| 巨大キーワードDB | GSC実データ+推定にとどめる |
| 事業数値のGitHub掲載 | OSS指標のみ、MRR等は非公開メモ |
| 週20時間超過 | Part 9厳守、並行は稼働増ではない |

---

## 16. バージョン管理

- v1.0: 2026年6月7日 初版(全ステージ統合、mos-kit抽出を依存順に組込)
- 更新トリガー: 各ステージ完了時に「現在地」(Section 1)を更新

---

**末尾メモ:**

このマスター書は、上から順に実行すれば3カテゴリ完成とその先の統合まで手が止まらないように作った。重要なのは順序。SEO編を公開・安定させ、mos-kitに共通基盤を抽出してから広告・SNSに展開する。この順序を守れば、3編へのコード重複はゼロ、自動投稿・自動入稿は構造的に不可能、という状態が自然に保たれる。

速くてよい。ただし順序を崩さず、境界(診断・評価まで)を越えない。

迷ったら、依存順に。境界を越えない。
