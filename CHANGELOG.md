# Changelog

Manifesto リポジトリの主な変更を記録する。透明性原則（第 4 章）に基づき、ロードマップの変更は理由を添えて記載する。

This file records notable changes to the Manifesto repository. Per the transparency principle (Chapter 4), roadmap changes are recorded together with their rationale.

---

## 2026-07-04

- 第 2.5 章「マーケAIの地図 / The Map of AI Marketing」を追加。理由: 三本柱（第 2 章）と境界線（第 3 章）の間に、マーケティング AI ツール群全体の中での自らの位置を示す層が欠けていたため。「地図を描いてから、自分たちの位置を示す」流れに揃えた。固有のサービス名を挙げない分類学（実行型・生成型・観測型・判断構造化型）として記述している。
  - Added Chapter 2.5 "The Map of AI Marketing." Rationale: between the Three Pillars (Ch. 2) and the Boundary (Ch. 3), the Manifesto lacked a layer showing where we stand within the broader landscape of AI marketing tools. The chapter is written as a taxonomy without service names (execution / generation / observation / decision-structuring).
- ロードマップ（第 5 章）に「現在地（2026年7月）」と次フェーズ（E3 横断 docs サイト＝任意、E2 共通 UI 抽出＝任意、コミュニティ運用の継続）を明記。Phase 5〜7・統合 CLI の前倒し理由（mos-kit 抽出と実装の並列化による土台の再利用）を追記。
  - Clarified "where we are (July 2026)" and the next phases (E3 cross-repo docs site — optional; E2 shared web UI — optional; ongoing community operations) in the Roadmap chapter, and recorded why Phases 5–7 and the unified CLI shipped early (mos-kit extraction and parallelized implementation).
- 第 6 章に「地図への貢献」の節を追加。型の追加提案・境界事例の報告を Issue で受け付ける一方、個別サービス名のカタログ化は行わない方針を明文化。
  - Added a "Contributing to the Map" note to Chapter 6: type proposals and boundary cases are welcome via Issues, while cataloging individual service names is explicitly out of scope.
- `CHANGELOG.md` を新設。以後、ロードマップ・本文の変更記録はここに集約する。
  - Established this `CHANGELOG.md`; roadmap and body changes are recorded here from now on.

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
