# Start-X OSS 次フェーズ実装指示書 — Manifesto同期 + 運用完成度 v2.0

**作成日:** 2026年6月21日
**前提となる現状:** N5〜N9・N8・BYOK・Yahoo/GSC 連携・QUICKSTART・npm 一括公開まで完了。`master_roadmap_v3.md` とカテゴリ README が実態より遅れている。
**横断制約:** 運用マニュアル Part 9 v1.1（週20h上限・攻撃的語彙禁止・事業数値非公開）
**実行:** Cursor(Agent) + ローカル `pnpm` / `gh` / `wrangler`

---

## 0. 現状把握（GitHub / npm 事実ベース・2026-06-21）

| リポ / 成果物 | 実体 | manifesto / master の記述 | ズレ |
|---|---|---|---|
| `@start-x-work/mos-seo` | 1.1.1 npm | Phase 3 のみ言及が多い | BYOK・GSC BYOK・QUICKSTART 未記載 |
| `@start-x-work/mos-ads` | **0.1.2** npm（Yahoo platform CLI） | v0.1.0 表記が残存 | Yahoo 実装・BYOK Web 未同期 |
| `@start-x-work/mos-social` | 0.1.1 npm + Web N8 | social README に Web URL なし | 軽微 |
| `@start-x-work/marketing-os` | **0.1.1** npm | master §9「npm 待ち」 | **未同期** |
| 各 Web UI | BYOK（sessionStorage） | 運営 Secrets 前提の古い記述 | 要更新 |
| `docs/QUICKSTART.md` | 5 リポ + manifesto ハブ | manifesto 索引に未登録（n2 で追加） | 要同期 |

### 0-1. なぜこれが「次の実装」になるか

Manifesto 第4章（透明性）: ロードマップは公開し、変更理由を可能な限り説明する。実装済みを古い予定のまま見せない。

→ **Task A: manifesto / master_roadmap / カテゴリ README の現実同期**
→ **Task B: N10 商用導線の完了確認 + 利用者導線（QUICKSTART ハブ）の索引化**

N10 の CLI フッター・Web フッター・README 表は概ね実装済み。本フェーズでは「ドキュメント上の完了宣言」と残差分の埋めを行う。

---

## 全体構成

```
Task A  manifesto + master_roadmap + カテゴリ README 同期   ← 先に片付ける
Task B  N10 完了確認 + QUICKSTART 索引 + 発展タスク(E3)の位置づけ明記
```

---

# Task A — manifesto / master ロードマップの現実同期

## A-1. ゴール

README 第5章・`master_roadmap_v3.md`・各 `seo|ads|social/README.md` を 2026-06-21 時点の実態に合わせる。

## A-2. 追記・更新する事実（煽らず淡々と）

| 項目 | 記載内容 |
|---|---|
| 統合 CLI | N9 完了。`@start-x-work/marketing-os@0.1.1`（mos-ads@0.1.2 依存） |
| BYOK | 全 Web UI で AI API キーをブラウザ sessionStorage に保存。運営側 AI Secrets 不要 |
| GSC | SEO Web で利用者自身の OAuth アプリ（BYOK）。`/gsc-callback` |
| Yahoo Ads | Ads CLI `platform yahoo` + Web BYOK。読み取り専用 |
| QUICKSTART | [docs/QUICKSTART.md](./docs/QUICKSTART.md) を横断入口としてリンク |
| npm | mos-kit 0.1.0 / mos-seo 1.1.1 / mos-ads 0.1.2 / mos-social 0.1.1 / marketing-os 0.1.1 |

## A-3. 受け入れ条件

- [ ] `manifesto/README.md` 第5章に BYOK・GSC・Yahoo・統合 CLI npm 版を反映
- [ ] `master_roadmap_v3.md` §2・§4・§9 を現状に更新（古い「npm 待ち」削除）
- [ ] `seo|ads|social/README.md` に Web URL・BYOK・QUICKSTART リンク
- [ ] 事業数値・攻撃的語彙なし

---

# Task B — N10 完了確認 + 利用者導線

## B-1. ゴール

N10（商用導線）の受け入れ条件を満たしていることをドキュメント上で明示し、発展タスク E3（docs サイト）は「任意・未着手」と Coming Soon 扱いにする。

## B-2. N10 チェックリスト

- [x] mos-kit `COMMERCIAL_HINT`
- [x] 各編 CLI 出力末尾（`--quiet` 抑制可）
- [x] 各編 Web フッター → marketing-os.jp
- [x] 各編 README OSS/商用 表
- [ ] `remaining_nodes_completion_v1.md` Part F のチェックボックス更新

## B-3. 発展タスク（任意・本フェーズでは実装しない）

| ID | 内容 | 状態 |
|---|---|---|
| E1 | CI 標準化 | 各リポ個別 CI 済み。テンプレ集約は任意 |
| E2 | mos-kit-web UI 共有化 | 3 編 Web 完成後の磨き込み |
| E3 | 横断 docs サイト | **未着手**（QUICKSTART.md 群で代替中） |
| E4 | テンプレートリポ | 構想拡張時 |

---

## 進捗マップ（本書実行後の到達点）

```
✓ N0〜N9        完了（npm 含む）
✓ N7/N8 Web     完了 + BYOK
✓ BYOK/GSC/Yahoo 完了
✓ QUICKSTART    5 リポ + manifesto ハブ
◐ A             manifesto/master 同期（本書 Task A）
◐ B             N10 ドキュメント完了宣言（本書 Task B）
▷ E3            docs サイト（任意・次フェーズ候補）
```

---

## Cursor プロンプト（Agent 用）

```
@next_phase_manifesto_sync_n2_v1.md の Task A と Task B を実行してください。

Task A: manifesto/README.md、master_roadmap_v3.md、seo|ads|social/README.md を
2026-06-21 実態（BYOK、GSC/Yahoo、npm 版、QUICKSTART）に同期。

Task B: remaining_nodes_completion_v1.md Part F のチェックボックスを現状に合わせて更新。
E3 docs サイトは未着手のまま明記。

制約: 事業数値なし、攻撃的語彙なし、自動投稿/入稿/最適化の追加なし。
完了後、変更ファイル一覧を提示。
```

---

## バージョン

- v1.0: 2026年6月21日。N5〜N9・BYOK・Yahoo・QUICKSTART 完了後の manifesto 同期 + N10 完了宣言。
