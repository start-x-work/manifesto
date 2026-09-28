# Changelog

Manifesto リポジトリの主な変更を記録する。透明性原則（第 4 章）に基づき、ロードマップの変更は理由を添えて記載する。

This file records notable changes to the Manifesto repository. Per the transparency principle (Chapter 4), roadmap changes are recorded together with their rationale.

---

## 2026-09-28（N1：商用側での取り込み・開発ブランチまで）

- ロードマップ（第 5 章・日英）の「SEO 編 v1.2（N1）」に、商用 Marketing-OS 側の取り込み（N1-4）の状況を追記した。監査ロジックの複製を、`@start-x-work/marketing-os-seo-core` の `./geo`（版は 1.2.0 に固定）への npm 依存に置き換える変更が、開発ブランチまで統合された。理由：OSS 側で正本化した監査ロジックが、商用側で実際に使われ始めた段階を、透明性原則に基づいて示すため（manifesto 指示書 M1）。
  - 明記した点：**本番環境にはまだ反映されていない**ため、「提供中」「本番で稼働」とは書いていない。本番に反映されたら、日付とともに改めて記録する。商用リポは非公開のため、PR 番号やリンクは載せていない。
  - 同じ日の前のエントリにある「商用側の取り込み（N1-4）は Marketing-OS リポで進行中」は、その時点の記録としてそのまま残した。
- Added the status of the commercial-side adoption (N1-4) to "SEO pillar v1.2 (N1)" in the Roadmap (Ch. 5, JP/EN). The change that replaces the commercial Marketing-OS's copy of the audit logic with an npm dependency on `@start-x-work/marketing-os-seo-core`'s `./geo` (pinned at 1.2.0) has been merged into the development branch. Rationale: per the transparency principle, show the stage at which the audit logic canonicalized on the OSS side is actually being used by the commercial product (manifesto spec M1).
  - Noted explicitly: it has **not yet reached production**, so we do not describe it as available or live; we will record the production date separately when it ships. The commercial repository is private, so no PR numbers or links are included. The earlier same-day note saying N1-4 was "in progress" is kept as the record at that time.

## 2026-09-28（SEO 編 v1.2 の反映）

- ロードマップ（第 5 章・日英）に「SEO 編 v1.2（N1）」を追加。理由：`@start-x-work/mos-seo` 1.2.0 と `@start-x-work/marketing-os-seo-core` 1.2.0（ライブラリの初公開）を npm に公開し、公開状態とロードマップを一致させるため（manifesto 指示書 M1、marketing-os-seo 指示書 N1-5）。
  - 内容：監査ロジックの正本を OSS 側に集約し（GEO／LLMO 監査は `./geo` サブパス）、商用 Marketing-OS は npm 依存として取り込む。あわせて、公開 HTTPS に限る安全な取得（SSRF 対策）、AI クローラの「検索・引用系／学習系」の区分、`Content-Signal` の解析、`llms.txt` の草案出力（命令ではなく案内）を追加した。
  - 明記した点：商用側の取り込み（N1-4）は Marketing-OS リポで進行中で、本更新は OSS 側の公開状態だけを反映する。
  - Added "SEO pillar v1.2 (N1)" to the Roadmap (Ch. 5, JP/EN). Rationale: `@start-x-work/mos-seo` 1.2.0 and `@start-x-work/marketing-os-seo-core` 1.2.0 (first publish of the library) are on npm, so the roadmap is brought in line with the published state (manifesto spec M1; marketing-os-seo spec N1-5). The commercial-side adoption (N1-4) is in progress in the Marketing-OS repository; this update reflects only the OSS publication state.

## 2026-09-27（manifesto 実装指示書 v1.1：M1・M2・M3）

- **M1（ロードマップ実態同期）** ロードマップ（第 5 章・日英）に需要予測 `forecast-manifesto` を「設計と編集」シリーズ第2弾として追加（公開済み・継続開発、`@forecast-manifesto/*`、Apache-2.0、外部依存ゼロ・シード固定の計算方法を公開）。「現在地」にも追記。理由: 実態（npm 公開・継続開発）をロードマップに反映するため（透明性原則）。SEO v1.0・read専用 MCP・広告/SNS の記述は現状のまま（実態と一致）。
  - **M1 (roadmap actual-state sync).** Added demand forecasting `forecast-manifesto` to the Roadmap (Ch. 5, JP/EN) as volume two of the "design and editing" series (published, in active development; `@forecast-manifesto/*`, Apache-2.0; zero-dependency, fixed-seed methods). Also noted in "where we are." Rationale: reflect the actual published state on the roadmap (transparency principle).
- **M2（地図章 2.5 の改訂）** 第 2.5 章に新節 **2.5.3「二つの観点 / Two Lenses」** を追加（媒体を売る型／売らない型、計算の中身を公開する型／しない型）。以降の節を繰り下げ（型の組み合わせ→2.5.4、私たちの位置→2.5.5、確かめる問い→2.5.6）。「私たちの位置」に、媒体を売らない側・計算を公開する側に立つこと、および承認済み判断のタスク受け渡し（第 3 章への橋）を追記。理由: 差別化の再定義（利害と情報の所在を構造として示す）。固有名・比較・数値主張は入れず、他の型への優劣断定もしない。
  - **M2 (Map chapter 2.5 revision).** Added a new **2.5.3 "Two Lenses"** subsection (selling-the-medium vs not; computation-open vs not), renumbering the rest (Combining Types → 2.5.4, Where We Stand → 2.5.5, Questions → 2.5.6). "Where We Stand" now states our not-selling / computation-open position and bridges to the Chapter-3 task-handoff. Rationale: restate differentiation as a structure (where interests and information sit); no service names, comparisons, numeric claims, or superiority judgments.
- **M3（境界章の改訂・D-13 の開示）** 第 3 章に、承認済みの判断に限り既存のタスク管理（チャット・課題管理）へ「タスク」として渡すことを境界の内側として明示。**広告・MA・CRM のデータ／設定の書き換えは承認の有無にかかわらず恒久的に行わない**旨を明記。改訂前は書き戻しを一切扱っていなかった旨も本文に残した。理由: 判断と実行の間の手作業の断絶を減らしつつ媒体中立を保つ（境界の変更のため、透明性原則により開示が必須）。注記: この境界に対応する実装（商用側のタスク連携）は今後のフェーズであり、本改訂は決定済みの境界を透明性原則に基づいて開示するもので、未実装機能の宣伝ではない。
  - **M3 (Boundary chapter revision — disclosing D-13).** Chapter 3 now states that, for human-approved decisions only, handing a decision to an existing task manager (chat / issue tracker) as a "task" is inside the boundary, while **changing data or settings in ad, MA, or CRM systems remains permanently out of bounds, approved or not.** The text preserves that earlier wording did not address write-backs. Rationale: reduce the manual gap between deciding and executing while keeping medium-neutrality; a boundary change must be disclosed per the transparency principle. Note: the implementation corresponding to this boundary is a later phase; this revision discloses a *decided boundary* per the transparency principle and is not the advertising of an unbuilt feature.

## 2026-09-13

- ロードマップ（第 5 章・日英）に、クリエイティブ編（`mos-creative` v0.1・コード公開）と動画編（`mos-video` v1.0・公開）を新ノードとして追加。「現在地」を 2026年9月に更新し、次フェーズに「mos-creative の npm 公開」を追記。理由: 両リポジトリを 2026 年 9 月に公開したため、透明性原則（第 4 章）に基づき公開ロードマップへ反映する。
  - Added the Creative pillar (`mos-creative` v0.1, code public) and the Video pillar (`mos-video` v1.0, published) as new nodes in the Roadmap (Ch. 5, JP and EN). Updated "where we are" to September 2026 and added "npm publish of mos-creative" to the next phases. Rationale: both repositories were published in September 2026; per the transparency principle (Ch. 4), we record them on the public roadmap.
  - 明記した点: `mos-creative` は現時点でコード公開のみ（npm 公開は準備中）、`mos-video` は自動投稿を持たない設計（実行系を追加しない境界＝各リポの `docs/12_why_no_autopost.md`）。実態に反する完了・自動化表現は用いていない。
  - Noted explicitly: `mos-video` has no auto-posting by design (the boundary of not adding an execution layer — see each repo's `docs/12_why_no_autopost.md`). We avoid completion or automation claims that would misrepresent the actual state.
- `mos-creative` を npm 公開（`@start-x-work/mos-creative@0.1.0`, Apache-2.0）。ロードマップ・本文の「npm 準備中／コード公開のみ」表記を「npm 公開済み」に更新し、次フェーズ一覧から「mos-creative の npm 公開」を削除。理由: 実際に公開が完了したため、表記を実態に合わせる。
  - Published `mos-creative` to npm (`@start-x-work/mos-creative@0.1.0`, Apache-2.0). Updated the roadmap/body wording from "npm pending / code-public only" to "published on npm" and dropped "npm publish of mos-creative" from the next-phase list. Rationale: the publish is now done, so the wording is brought in line with reality.

## 2026-07-09

- ロードマップ（第 5 章）と `master_roadmap_v3.md` に「エージェント連携（read専用・MCP）」を次フェーズの計画中ノードとして追加。理由: 商用 Marketing-OS 側で MCP サーバー実装（OAuth 2.1 + PKCE・readOnly-first・書き込みツールを型レベルで定義しない設計）が次の実装対象として起票されたため、着手前の段階から公開ロードマップに記載する（透明性原則）。
  - Added "Agent integration (read-only, MCP)" to the Roadmap (Ch. 5) and `master_roadmap_v3.md` as a planned, not-yet-started next-phase node. Rationale: an MCP server implementation (OAuth 2.1 + PKCE, readOnly-first, write tools never defined at the type level) has been proposed as the next build item on the commercial Marketing-OS side; per the transparency principle, we record it on the public roadmap before work begins.
  - 明記した点: OSS 三本柱（SEO/広告/SNS）の診断 CLI・Web は npm 公開・稼働確認済みのままであり、本更新はこれらの完了状態を変更しない。別リポ（商用 Marketing-OS）側の起票情報を理由に、検証済みの公開実績を「準備中」へ書き換えることはしていない。
  - Noted explicitly: the three OSS pillars' diagnostic CLI/Web remain published on npm and verified working; this update does not change their completed status. We did not roll back a verified public shipping status to "in preparation" on the strength of a proposal for a separate (commercial) repository.

## 2026-07-04

- 第 2.5 章「マーケAIの地図 / The Map of AI Marketing」を追加。理由: 三本柱（第 2 章）と境界線（第 3 章）の間に、マーケティング AI ツール群全体の中での自らの位置を示す層が欠けていたため。「地図を描いてから、自分たちの位置を示す」流れに揃えた。固有のサービス名を挙げない分類学（実行型・生成型・観測型・判断構造化型）として記述している。
  - Added Chapter 2.5 "The Map of AI Marketing." Rationale: between the Three Pillars (Ch. 2) and the Boundary (Ch. 3), the Manifesto lacked a layer showing where we stand within the broader landscape of AI marketing tools. The chapter is written as a taxonomy without service names (execution / generation / observation / decision-structuring).
- ロードマップ（第 5 章）に「現在地（2026年7月）」と次フェーズ（E3 横断 docs サイト＝任意、E2 共通 UI 抽出＝任意、コミュニティ運用の継続）を明記。Phase 5〜7・統合 CLI の前倒し理由（mos-kit 抽出と実装の並列化による土台の再利用）を追記。
  - Clarified "where we are (July 2026)" and the next phases (E3 cross-repo docs site — optional; E2 shared web UI — optional; ongoing community operations) in the Roadmap chapter, and recorded why Phases 5–7 and the unified CLI shipped early (mos-kit extraction and parallelized implementation).
- 第 6 章に「地図への貢献」の節を追加。型の追加提案・境界事例の報告を Issue で受け付ける一方、個別サービス名のカタログ化は行わない方針を明文化。
  - Added a "Contributing to the Map" note to Chapter 6: type proposals and boundary cases are welcome via Issues, while cataloging individual service names is explicitly out of scope.
- `CHANGELOG.md` を新設。以後、ロードマップ・本文の変更記録はここに集約する。
  - Established this `CHANGELOG.md`; roadmap and body changes are recorded here from now on.
- OSS 参加動線の受け皿を整備: `CONTRIBUTING.md`・`CODE_OF_CONDUCT.md`（Contributor Covenant 2.1）・`SECURITY.md` を新設。第 4 章（Contributor Covenant 遵守・セキュリティ報告への応答）と第 6 章（参加方法）の約束を実ファイルで裏付けた。あわせて「地図への貢献」を受ける Issue テンプレート（`.github/ISSUE_TEMPLATE/map_contribution.yml`）を追加。
  - Added the receiving surfaces for OSS participation: `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md` (Contributor Covenant 2.1), and `SECURITY.md`, backing the promises in Chapters 4 and 6 with real files. Added an Issue template (`.github/ISSUE_TEMPLATE/map_contribution.yml`) to receive "contributions to the map."
- `master_roadmap_v3.md` を同期: ドキュメント体系表に `CHANGELOG.md` と第 2.5 章を登録し、バージョン管理に v3.2 を追記（理由付き）。索引が本文更新に追随しない無言の陳腐化を防ぐ。
  - Synced `master_roadmap_v3.md`: registered `CHANGELOG.md` and Chapter 2.5 in the document table and added a v3.2 version entry (with rationale), so the index does not silently fall behind the body.

## 2026-06（さかのぼり記録 / retrospective）

CHANGELOG 新設以前の変更を、コミット履歴からさかのぼって記録する。
Changes prior to this file's creation, reconstructed from commit history.

- Phase 3（SEO CLI）・Phase 4（SEO Web UI）を「完了（前倒し）」に更新。当初想定（7〜10 月）に対し 2026 年 6 月に完了。理由: SEO 編の実装が想定より順調に進んだため。
  - Marked Phase 3 (SEO CLI) and Phase 4 (SEO Web UI) complete, ahead of the original July–October window (shipped June 2026). Rationale: SEO implementation progressed faster than expected.
- Phase 5（SEO v1.0 + 広告準備）・Phase 6（広告 v0.1）・Phase 7（SNS v0.1）・統合 CLI（N9）の完了を順次反映。npm 公開版: mos-kit 0.1.0 / mos-seo 1.1.1 / mos-ads 0.1.2 / mos-social 0.1.1 / marketing-os 0.1.1。
  - Synced completion of Phase 5 (SEO v1.0 + Ads prep), Phase 6 (Ads v0.1), Phase 7 (Social v0.1), and the unified CLI (N9). Published on npm: mos-kit 0.1.0 / mos-seo 1.1.1 / mos-ads 0.1.2 / mos-social 0.1.1 / marketing-os 0.1.1.
- BYOK 運用設計（AI キー・GSC OAuth・Yahoo トークンを利用者ブラウザの sessionStorage に保存）と利用者向け QUICKSTART（docs/QUICKSTART.md）を反映。
  - Recorded the BYOK operating model (AI keys, GSC OAuth, and Yahoo tokens stored in the user's browser sessionStorage) and the user-facing QUICKSTART hub (docs/QUICKSTART.md).
- 商用サービス構成（AI CMO / BPO 並列、プラン=クォータ・機能形状=ビジネスタイプ）の記述を第 3 章に同期。
  - Aligned Chapter 3 with the current commercial service structure (AI CMO and BPO as parallel offerings; plans set quotas, business type sets feature shape).

## 2026-05

- Manifesto 初版公開（6 章構成: Why / 3 つの柱 / 境界線 / 原則 / ロードマップ / 参加方法）。
  - Initial publication of the Manifesto (six chapters: Why / Three Pillars / Boundary / Principles / Roadmap / Contribute).
