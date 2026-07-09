# Changelog

Manifesto リポジトリの主な変更を記録する。透明性原則（第 4 章）に基づき、ロードマップの変更は理由を添えて記載する。

This file records notable changes to the Manifesto repository. Per the transparency principle (Chapter 4), roadmap changes are recorded together with their rationale.

---

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
