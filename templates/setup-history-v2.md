# Start-X OSS Phase 1 実施記録（指示書 v2.0 準拠）

**記録更新:** 2026-05-25  
**参照指示書:** Start-X OSS Phase 1 実施指示書 v2.0（2026-05-13）

## 完了した主項目

| 指示書セクション | 内容 | 状態 |
|------------------|------|------|
| 2-1 | Organization `start-x-work` プロフィール等 | 実施済み。2FA 必須化のみ GitHub 側で未反映 |
| 2-2 | 5 リポジトリ | 作成済み |
| 2-3 | `.github` Org テンプレート | 配置・push 済み |
| 2-4〜2-6 | manifesto / seo / ads / social scaffold → 本文化 | README 6 章・editor・licenses・カテゴリ 3 本まで拡張済み |
| 2-7 | Discussions / Wiki・Projects 無効 / Topics / main 保護 | 実施済み（保護は Public 化後に API 適用） |
| 2-8 | LLMO 診断抽出調査 | `開発/start-x-work/research/extraction-feasibility.md`（ローカル）および本記録 |
| 3〜4 | Manifesto 執筆・カテゴリ Manifest・実装 README 接続 | 実施済み |
| 4-8 | 全リポ Public 化 | 実施済み（無料プランのブランチ保護のため前倒し） |
| Discussion | 最初の Organization 連携 Discussion | [manifesto/discussions/1](https://github.com/start-x-work/manifesto/discussions/1) |

## Phase 1 完了前の外部設定

| 項目 | 備考 |
|------|------|
| Org の **2FA 必須**（指示書 2-1） | GitHub API で `two_factor_requirement_enabled=true` を適用したが、確認値は `false` のまま。Organization owner の `admin:org` 権限・アカウント2FA・ブラウザ確認が必要 |
| メンバー権限デフォルト（Read / Private repo 作成制限） | API 確認済み。`default_repository_permission=read`、repo 作成は `none`（この Org では private-only が許可されず、より厳格に public/private とも作成不可へ設定） |
| **付録E** 各媒体の**公開ボタン**（note / X / Threads / theLetter / Facebook / LinkedIn） | ユーザー報告により公開済み。下書きは [announcement-drafts/](./announcement-drafts/) |
| 初週 **Star** 50 目標 | `gh repo star start-x-work/<name>` または UI |
| **MRR ¥500k** 本体への影響 | 経営判断（指示書 5-1 必須） |

## 付録D（メトリクス・撤退線）

数値目安と Soft/Hard 停止条件は指示書 **付録D** の表を正とする。初回値と週次記録は [metrics.md](./metrics.md) に残す。

## 指示書 §12（完了後の確認）

| Step | 内容 | 状態 |
|------|------|------|
| 1 | `gh repo view start-x-work/<repo>` で 5 本確認 | 自動確認可能 |
| 2 | Organization ページ `https://github.com/start-x-work` | ブラウザ |
| 3 | 他リポで Issue テンプレが Org デフォルトを継承しているか | ブラウザ |
| 4 | Manifesto 相当ドキュメント一式 | README・editor・licenses・カテゴリ 3・本 templates |
| 5 | 公開アナウンス 6 媒体 | ユーザー報告により公開済み。原稿アーカイブは `announcement-drafts/` |
| 6 | Phase 2（6 月）へ移行する認識 | [Issue #3](https://github.com/start-x-work/manifesto/issues/3) に引き継ぎ |

