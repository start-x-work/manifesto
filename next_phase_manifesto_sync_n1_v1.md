# Start-X OSS 次フェーズ実装指示書 — Manifesto同期 + N1(SEO v1.0)v1.0

**作成日:** 2026年6月7日
**前提となる現状:** N0(SEO CLI)・N2(SEO Web)公開完了。manifestoのロードマップが実態より3〜4ヶ月遅れた記述のまま。
**横断制約:** 運用マニュアル Part 9 v1.1(週20h上限・攻撃的語彙禁止・事業数値非公開)
**実行:** Cursor(Agent)+ ローカル `pnpm` / `gh` / `wrangler`

---

## 0. 現状把握(GitHub事実ベース・2026-06-07)

| リポ | 実体 | manifestoロードマップの記述 | ズレ |
|---|---|---|---|
| `manifesto` | 12コミット・公開済み | — | ロードマップが古い |
| `marketing-os-seo` CLI | **v0.1.0 npm公開 + Release済み** | 「Phase 3(7–8月)でCLI」 | **約2ヶ月前倒し** |
| `marketing-os-seo` Web | **Cloudflare Pages公開済み** | 「Phase 4(9–10月)でWeb」 | **約3〜4ヶ月前倒し** |
| `marketing-os-ads` | scaffold | 「Phase 6(2027 Q1)で着手」 | 予定通り未着手 |
| `marketing-os-social` | scaffold | — | 予定通り未着手 |

### 0-1. 検出した不整合(要対応)

manifestoのロードマップはこう書いている:
- Phase 3(7–8月)= SEO CLI
- Phase 4(9–10月)= Web UI

だが**両方とも本日(6月)時点で公開済み**。manifestoの記述が実態に追いついていない。

### 0-2. なぜこれが「次の実装」になるか

manifesto の原則(第4章)が、ロードマップと実態のズレを放置しないことを自ら約束している:
- 「ロードマップは公開し、変更理由を可能な限り説明する」
- 「日程は前提であり、変更されうる。判断はロードマップ上で明示する」
- 「Coming Soon は明示し、未実装を曖昧に見せない」(裏返せば、実装済みを古い予定のまま見せるのも不誠実)

→ **ロードマップの現実同期は、manifesto自身のルールが要求する作業。** これを Task A とする。
→ 並行して、クリティカルパス継続の **N1(SEO v1.0)を Task B** とする。

---

## 全体構成

```
Task A  manifesto ロードマップの現実同期(透明性原則の履行)   ← 軽量・先に片付ける
Task B  N1(SEO v1.0:安定API + v0.2機能)                      ← クリティカルパス継続
```

A と B は別リポなので並行可。A は短時間で終わるので先に。

---

# Task A — manifesto ロードマップの現実同期

## A-1. ゴール

manifesto README の第5章(ロードマップ)を、実態に合わせて更新し、**前倒しした事実と理由を明示**する。Part 9 の「事業数値を出さない」を守り、MRR等の数値は書かない。

## A-2. 修正方針

| 観点 | 方針 |
|---|---|
| Phase 3(CLI) | 「完了(2026年6月)」に更新 |
| Phase 4(Web) | 「完了(2026年6月)」に更新 |
| 前倒しの理由 | 「実装が想定より順調に進み、前倒しで公開した」旨を1〜2文。煽らず淡々と |
| Phase 5以降 | 残予定として維持(広告編Manifest詳細化・SEO v1.0準備など) |
| 数値 | MRR・顧客数等は書かない(Part 9。manifestoも「別紙の運用基準に委ねる」としている) |
| 表現 | 攻撃的語彙(駆逐・破壊・覇権等)禁止。「正直に到達度を示す」トーン |

## A-3. 修正後のロードマップ(日本語・案)

```markdown
## 5. ロードマップ

Phase 1(2026年5月):OSS 基盤と Manifesto の公開 — 完了。
Phase 2(6月):商用 Marketing-OS 本体への集中。OSS は最小メンテナンス。
Phase 3(SEO 編 v0.1 CLI):完了(2026年6月、当初想定より前倒し)。
  @start-x-work/mos-seo として npm 公開。LLMO/AEO 診断・サイト診断・
  コンテンツブリーフ・キーワード意図マッピングを CLI で提供。
Phase 4(SEO 編 Web UI):完了(2026年6月、当初想定より前倒し)。
  Cloudflare 上で公開。CLI と同じ診断を Web から利用可能。
Phase 5(SEO v1.0 + 広告編準備):進行中。
  SEO の公開 API 安定化、ボリューム推定・多言語・マルチモデル対応。
  並行して広告編 Manifest の詳細化と API 調査。
Phase 6(広告編 v0.1):未着手。SEO 編で確立した共通基盤を再利用して着手予定。

日程は前提であり変更されうる。前倒し・後ろ倒しいずれの場合も、
判断の理由をこのロードマップ上で明示する。今回 Phase 3・4 を前倒しできたのは、
SEO 編の実装が想定より順調に進んだためである。
```

英語版(第5章 English)も同様のトーンで同期する。

## A-4. タスク

| # | タスク | 種類 |
|---|---|---|
| A-T1 | README.md 第5章(日本語)を A-3 に沿って更新 | `[Cursor]` |
| A-T2 | README.md 第5章(English)を同期 | `[Cursor]` |
| A-T3 | seo/README.md があれば「実装済み・リンク」を反映 | `[Cursor]` |
| A-T4 | コミット & push | `[CLI]` |

## A-5. 実行

`【Cursor】`(manifestoリポを開いて)
```
README.md の第5章ロードマップ(日本語・English両方)を、実態に同期します。
現状:SEO CLI(@start-x-work/mos-seo v0.1.0)と SEO Web UI は2026年6月時点で公開済み。
manifestoの記述「Phase 3=7-8月CLI / Phase 4=9-10月Web」は古いので、両方「完了(2026年6月、前倒し)」に更新。
前倒しの理由(実装が順調)を1-2文添える。

制約:
- MRR・顧客数などの事業数値は一切書かない(別紙運用基準に委ねる、というmanifestoの方針を維持)
- 攻撃的語彙(駆逐/破壊/覇権等)を使わない。淡々と到達度を示すトーン
- Phase 5以降は残予定として維持
- 日本語とEnglishでトーン・内容を揃える
- seo/README.md に「Coming Soon」表記が残っていれば「公開済み + リンク」に修正

変更後、差分を見せてください。コミットは私が確認してから。
```

`【ターミナル】`(確認後)
```bash
cd ~/projects/manifesto   # パスは実際の場所に
git add -A
git commit -m "docs: sync roadmap with actual progress (SEO CLI & Web shipped ahead of schedule)"
git push origin main
```

## A-6. 受け入れ条件

- [ ] 第5章が実態(CLI/Web公開済み)を反映
- [ ] 前倒しの理由が明記されている
- [ ] 事業数値が書かれていない
- [ ] 攻撃的語彙がない
- [ ] 日英で内容が揃っている

---

# Task B — N1(SEO v1.0:安定API + v0.2機能)

## B-1. ゴール

`marketing-os-seo` の core を **semver安定の公開API**にし、v0.2機能(ボリューム推定・多言語・マルチモデル)を追加して **v1.0.0** を公開。これが N4(mos-kit抽出)の前提になる。

## B-2. スコープ(系統A:詳細は phase5_ads_prep_seo_v1.md)

| # | 項目 | 内容 |
|---|---|---|
| A1 | 公開API確定 | `core/src/index.ts` に公開APIを集約。以降破壊的変更しない |
| A2 | ボリューム推定 | GSC実データ優先 + クラスタ相対推定。**巨大DBは作らない** |
| A3 | 多言語対応 | `--lang`(default ja) |
| A4 | マルチモデルLLMO | `createProvider` に openai / anthropic 追加、`--model` 切替 |
| A5 | Marketing-OS連携導線 | 出力末尾に控えめな商用リンク(Part 9:宣伝最小限) |
| A6 | v1.0.0 リリース | npm + Release |

## B-3. 実装核(型)

```typescript
// packages/core/src/index.ts — 公開API集約(安定化)
export { auditLLMO, type LLMOAuditResult, type LLMOCheck } from "./llmo/audit";
export { auditSite, type SiteAuditResult, type SiteCheck } from "./site/audit";
export { generateBrief, type ContentBrief, type BriefOptions } from "./content/brief";
export { mapKeywords, type KeywordNode, type Intent } from "./keyword";
export { estimateVolume, type VolumeEstimate } from "./keyword/volume";
export { createProvider, type AIProvider, type CompleteOptions } from "./ai";
export { CliError, FetchError, AIError } from "./errors";

// A2: ボリューム推定(巨大DB禁止)
export interface VolumeEstimate {
  keyword: string;
  estimatedVolume: number | null;
  source: "gsc" | "estimated";
  confidence: "high" | "medium" | "low";
}

// A3: 多言語
export interface BriefOptions { lang?: string }   // default "ja"

// A4: マルチモデル
export function createProvider(
  model: "gemini" | "openai" | "anthropic" = "gemini",
  apiKey?: string,
): AIProvider;
```

## B-4. タスク

| # | タスク | 種類 |
|---|---|---|
| B-T1 | core公開APIを index.ts に集約(A1) | `[Cursor]` |
| B-T2 | volume.ts 実装(A2、巨大DB作らない) | `[Cursor]` |
| B-T3 | `--lang` 対応(A3) | `[Cursor]` |
| B-T4 | openai/anthropic provider追加 + `--model`(A4) | `[Cursor]` |
| B-T5 | 商用導線を出力末尾に(A5、控えめ) | `[Cursor]` |
| B-T6 | テスト追加・全緑 | `[Cursor]` |
| B-T7 | version 1.0.0 に更新・公開(A6) | `[CLI]` |

## B-5. 実行

`【Cursor】`
```
@docs/phase5_ads_prep_seo_v1.md の系統A に従い、marketing-os-seo を v1.0 化します。

A1 公開API確定:core/src/index.ts に4機能 + volume + createProvider + errors を集約し、
   semver安定とする(以降破壊的変更しない方針をコメントで明記)。
A2 ボリューム推定:packages/core/src/keyword/volume.ts を新規。
   GSC実データがあれば優先、無ければクラスタ内相対推定 + confidence を返す。
   巨大なキーワードDBは作らない(GSC実データ + 推定にとどめる)。
A3 多言語:BriefOptions に lang(default "ja")、CLIに --lang。
A4 マルチモデル:createProvider に openai / anthropic を追加、CLIに --model。
   AIは必ず provider 経由(直叩き禁止)。APIキーは私が手動設定、.env.example のみ。
A5 商用導線:各コマンド出力末尾に1行
   「継続運用・チームでの意思決定には Marketing-OS → https://marketing-os.jp」
   --quiet で抑制可能に。煽らない。
A6 まだ公開しない:version を 1.0.0 に上げる準備までで、npm publish は私が実行。

制約:
- 本文の自動生成・自動投稿・自動最適化は実装しない(診断・評価・ブリーフまで)
- 事業数値をコードやREADMEに書かない
- テストはAIモックで実APIキー不要で緑になること
各タスクごとに commit。完了後、差分と次の公開コマンドを提示してください。
```

`【ターミナル】`(公開・私が実行)
```bash
cd ~/projects/marketing-os-seo
pnpm lint && pnpm build && pnpm test       # 全緑
# package.json の version を 1.0.0 に(cli/core両方)
cd packages/cli && npm publish --otp=（6桁 or トークン方式）
cd ~/projects/marketing-os-seo
git tag v1.0.0 && git push origin v1.0.0
gh release create v1.0.0 --repo start-x-work/marketing-os-seo \
  --title "SEO Toolkit v1.0.0" \
  --notes "Stable public API. Adds volume estimation, multi-language (--lang), multi-model LLMO (--model: gemini/openai/anthropic)."
npx @start-x-work/mos-seo@latest keyword map "ai marketing" --volume --format json
```

## B-6. 受け入れ条件

- [ ] core 公開APIが index.ts に集約・安定化
- [ ] `keyword map --volume` 動作(巨大DBなし)
- [ ] `--lang` 動作
- [ ] `--model gemini|openai|anthropic` 動作
- [ ] 商用導線が出力末尾(--quiet抑制可)
- [ ] 自動生成/最適化が存在しない
- [ ] v1.0.0 公開 + Release
- [ ] テスト全緑

---

## 並行メモ:N3(広告調査)

N1実装と並行して、N3(広告API調査・コード不要)を別タブのCursorで走らせておくと、後の広告編(N5)着手時に待ちゼロ。プロンプトは前回提示済み。

---

## 進捗マップ(本書実行後の到達点)

```
✓ N0  SEO CLI公開        完了
✓ N2  SEO Web UI         完了
◐ A   manifesto同期      本書 Task A で完了
◐ N1  SEO v1.0           本書 Task B で完了 → v1.0.0
▷ N4  mos-kit抽出        N1完了後(次の要)
▷ N3  広告調査           並行可
▷ N5-N9                 順次
```

N1完了で、次はいよいよ **N4(mos-kit共通基盤抽出)**。SEO編の安定coreを共有パッケージ化し、広告・SNSをコピーなしで作る土台になる。N4の詳細指示書は [`remaining_nodes_completion_v1.md`](./remaining_nodes_completion_v1.md) Part A にある。

---

## Part 9 整合チェック(本書の全タスク共通)

- [ ] 週20h制約:本書のタスク量は範囲内(A=軽量、B=中)。神経系低下時は分割
- [ ] 攻撃的語彙なし(manifesto同期・商用導線とも)
- [ ] 事業数値をGitHub/npm/READMEに出さない
- [ ] 自動生成・自動実行・自動投稿・自動入稿・自動最適化を実装しない
- [ ] AIは抽象層(provider)経由
- [ ] 認証情報はユーザーが手動、Claudeに渡さない

---

## バージョン

- v1.0: 2026年6月7日。現状(N0/N2完了・manifestoロードマップ乖離)を反映。Task A(manifesto同期)とTask B(N1=SEO v1.0)を定義。
