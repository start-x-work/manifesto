# Start-X OSS マスターロードマップ v3.0

**作成日:** 2026年6月7日
**前バージョン:** phase2-6 統合実装指示書 v2.0
**v3.0の変更点:**
- 実進行速度を反映(Phase 1〜3が想定を大幅に上回る速度で完了)
- ロードマップ・スケジュールを実態ベースに全面書き替え
- Phase 5 / 6 の実装指示書を新規作成し、本書から参照
- 月固定スケジュールを完全廃止、実績 + 残作業ベースに

---

## 0. このドキュメントについて

本書は Start-X OSS 戦略全体の**マスターロードマップ**。各Phaseの詳細実装指示書は別ファイルに分離され、本書はそれらを束ねる索引兼進捗管理。

### ドキュメント体系

| ドキュメント | 対象 | 状態 |
|---|---|---|
| `repo_setup_v2.md` | Phase 1(Manifesto) | ✓ 完了 |
| `phase1_completion_v1.md` | Phase 1 残タスク | ✓ 完了 |
| `phase3_seo_cli_finalize_v1.md` | Phase 3 仕上げ | 進行中 |
| `phase4_seo_web_impl_v1.md` | Phase 4(Web UI) | 待機 |
| [`phase5_ads_prep_seo_v1.md`](./phase5_ads_prep_seo_v1.md) | Phase 5(広告準備+SEO v1.0) | 待機 |
| [`phase6_ads_impl_v1.md`](./phase6_ads_impl_v1.md) | Phase 6(広告 v0.1) | 待機 |
| [`phase7_social_impl_v1.md`](./phase7_social_impl_v1.md) | Phase 7(SNS v0.1) | 将来 |
| [`implementation_blueprint_v2.md`](./implementation_blueprint_v2.md) | Phase 3〜統合CLI/商用連携 | 実装ブループリント |
| [`remaining_nodes_completion_v1.md`](./remaining_nodes_completion_v1.md) | N4/N7/N8/N9/N10 詳細 | 独立詳細指示書 |
| [`n0_execution_runbook_v1.md`](./n0_execution_runbook_v1.md) | 環境準備〜N0(SEO CLI公開) | 実行手順書 |
| **`master_roadmap_v3.md`(本書)** | 全体索引・進捗 | — |

---

## 1. 実績ベースの進捗(2026年6月7日時点)

実際の進行は当初計画(月単位)を大幅に上回った。以下は実績。

| Phase | 当初想定 | 実績 | 状態 |
|---|---|---|---|
| Phase 1: Manifesto | 5月の3週間 | 5/13 即日完了 | ✓ 完了 |
| Phase 1残: リンク/Issue/metrics | 5月内 | 完了 | ✓ 完了 |
| Phase 2: 初期運用 | 6月1ヶ月 | 短縮(数日) | ✓ 実質完了 |
| Phase 3: SEO CLI実装 | 7-8月(9-10週) | 5月内に実装 | ◐ コード完了・公開作業中 |
| Phase 4: SEO Web UI | 9-10月 | 前倒し可能 | 待機 |
| Phase 5: 広告準備+SEO v1.0 | Q4 | 前倒し可能 | 待機 |
| Phase 6: 広告 v0.1 | 2027 Q1 | 前倒し可能 | 待機 |

**観察:** Cursor + AI支援により、実装速度が当初想定の数倍。月固定スケジュールは無意味なので、本書では**「実績 + 残作業」**で管理する。

---

## 2. 更新後のロードマップ(実進行ベース)

```
✓ Phase 1   Manifesto公開 ............................ 完了
✓ Phase 1残 リンク/Issue/metrics ..................... 完了
✓ Phase 2   初期運用・安定化 .......................... 実質完了
◐ Phase 3   SEO CLI .................................. コード完了・公開作業中
   └─ 残: npm公開 + v0.1.0 Release(Gate B)
   │
▷ Phase 4   SEO Web UI ............................... 着手可能
   └─ Gate C: Web版がCloudflareで安定稼働
   │
▷ Phase 5   広告準備 + SEO v1.0 ...................... 着手可能(Phase 4と一部並行可)
   └─ Gate D: SEO v1.0 + 広告スコープ確定
   │
▷ Phase 6   広告 v0.1 ................................ 着手可能(Gate D後)
   └─ 完了: 広告編 v0.1 公開
   │
   (将来) SNS編(Phase 7) → 3カテゴリ完成 → メタブランド検討
```

凡例: ✓完了 / ◐進行中 / ▷待機(着手可能)

---

## 3. 更新後のスケジュール(スプリントベース)

日付固定を廃止。各Phaseは「着手したら完了まで一気に進む」スプリント単位。実速度に応じて並行・圧縮する。

| スプリント | 内容 | 並行可否 | 指示書 |
|---|---|---|---|
| **現在** | Phase 3 公開作業(Gate Bクローズ) | — | phase3_finalize |
| Sprint N+1 | Phase 4 Web UI | Phase 3完了後 | phase4_web |
| Sprint N+2 | Phase 5 系統A(SEO v1.0) | Phase 4と並行可 | [phase5](./phase5_ads_prep_seo_v1.md) |
| Sprint N+2 | Phase 5 系統B(広告準備) | Phase 4と並行可 | [phase5](./phase5_ads_prep_seo_v1.md) |
| Sprint N+3 | Phase 6(広告 v0.1) | Gate D後 | [phase6](./phase6_ads_impl_v1.md) |

### 並行実行の指針

実速度が速いため、以下は並行できる:

- **Phase 4(Web UI)と Phase 5系統B(広告準備・調査)** — Web UIはコード作業、広告準備は調査作業なので競合しない
- **Phase 5系統A(SEO v1.0)は Phase 4完了後** — coreの最終形が必要なので順序依存

ただし **Part 9 の週20時間制約は厳守**。並行は「同時に複数を進められる」という意味で、稼働時間を増やす意味ではない。

---

## 4. 次にやること(クリティカルパス)

### 最優先: Phase 3 Gate B クローズ

GitHub上に Release も npm公開も未反映(キャッシュでなく実体として未公開の可能性)。push済みでも、以下が残る:

```bash
cd ~/projects/marketing-os-seo
pnpm lint && pnpm build && pnpm test     # 緑確認
cd packages/cli && npm publish            # @start-x-work/mos-seo 公開
cd ~/projects/marketing-os-seo
git tag v0.1.0 && git push origin v0.1.0
gh release create v0.1.0 --repo start-x-work/marketing-os-seo \
  --title "SEO Toolkit v0.1.0 (CLI)" \
  --notes "First public release. 4 features. CLI only."
npx @start-x-work/mos-seo audit site https://example.com   # 外部確認
```

これで Phase 3 が客観的に完了する。

### その後の順序

1. **Phase 4(Web UI)着手** — `phase4_seo_web_impl_v1.md`
2. **Phase 5系統B(広告調査)を並行開始** — [`phase5_ads_prep_seo_v1.md`](./phase5_ads_prep_seo_v1.md) の B1〜B4
3. **Phase 4完了後、Phase 5系統A(SEO v1.0)** — A1〜A6
4. **Gate D通過後、Phase 6(広告実装)** — [`phase6_ads_impl_v1.md`](./phase6_ads_impl_v1.md)

---

## 5. Gate サマリー(全Phase)

| Gate | 通過条件 | 状態 |
|---|---|---|
| A(Phase 2→3) | 初期運用安定 + 実装準備完了 | ✓ 通過 |
| B(Phase 3→4) | 4機能CLI動作 + npm公開 + Release | ◐ 公開作業中 |
| C(Phase 4→5) | Web版がCloudflareで安定稼働 | 待機 |
| D(Phase 5→6) | SEO v1.0 + 広告スコープ確定 | 待機 |

すべて**OSS内部の完成度**で判定。事業数値には依存しない。

---

## 6. メトリクス(OSS指標のみ)

`manifesto/templates/metrics.md` で週次記録。

| 指標 | 追跡 |
|---|---|
| ★数(各リポ) | 全Phase |
| fork / watcher | 全Phase |
| Issue / Discussion | 全Phase |
| npm download(月間) | Phase 3〜 |
| Contributor数 | 全Phase |

具体的な数値目標・撤退ラインは**ローカルの非公開メモで管理**(本書・GitHub には記載しない)。

---

## 7. 横断的設計原則(全Phase不変)

### 7-1. Marketing-OS思想

| やる | やらない |
|---|---|
| 診断・評価・構造化 | コンテンツ自動生成 |
| 編集可能なテンプレート出力 | AI自律実行・自動投稿 |
| 意思決定の支援・記録 | 自動最適化・自動運用 |

広告編では interface 設計で構造的に排除(書き込みメソッドを定義しない)。

### 7-2. 技術的一貫性

- 全リポ TypeScript(strict)+ pnpm workspace
- AI は抽象層経由(`ai/provider.ts`、全編で流用)
- ビルド tsup / テスト vitest / Lint Biome で統一
- 広告編は SEO編の基盤をコピーして開始

### 7-3. ブランド一貫性

- ライセンス Apache 2.0(全リポ)
- 色 OS Indigo #5957EE + Slate #0A2540 + White
- NOTICE で Marketing-OS への導線保持
- 攻撃的語彙の禁止(駆逐/破壊/革命)

### 7-4. 認証情報

- APIキー・OAuthシークレットはユーザーが手動設定
- Cursor/Claude は扱わない・生成しない
- `.env` はコミットしない、`.env.example` のみ

### 7-5. Part 9 制約

- 週20時間上限(特例時のみ25h/最大8週間、事前申請)
- 並行実行は稼働時間を増やす意味ではない
- 神経系スコア低下時は即ペースダウン

---

## 8. 各実装指示書の使い方(Cursor)

| 今やること | 開く指示書 | Agent指示 |
|---|---|---|
| Phase 3公開 | phase3_finalize | Section 7のプロンプト |
| Phase 4実装 | phase4_web | Section 9のプロンプト |
| Phase 5実装 | [phase5](./phase5_ads_prep_seo_v1.md) | Section 6のプロンプト |
| Phase 6実装 | [phase6](./phase6_ads_impl_v1.md) | Section 7のプロンプト |

各指示書は独立して Cursor Agent に渡せる。本書(master)は全体の進捗管理用。

---

## 9. 完了の定義(OSS戦略 全体)

```
最終ゴール(3カテゴリ完成):
- [ ] SEO編 v1.0 公開(Phase 5)
- [ ] 広告編 v0.1 公開(Phase 6)
- [ ] SNS編 v0.1 公開(Phase 7)
- [ ] 3カテゴリを束ねる構想の検討

各編が「診断・評価・構造化」に徹し、Marketing-OS思想を保ったまま、
業界の生成AI化を促進し、Marketing-OS本体への導線として機能している状態。
```

---

## 10. バージョン管理

- v1.0: 2026年5月14日(Phase 2-6 統合、推奨技術)
- v2.0: 2026年5月14日(技術確定、Phase 3詳細化、事業数値削除)
- v3.0: 2026年6月7日(実進行反映、Phase 5/6を別ファイル化、ロードマップ・スケジュール全面更新)

---

**末尾メモ:**

当初の月単位スケジュールは、実際の進行速度の前で意味を失った。Phase 1は即日、Phase 3も短期間で実装まで到達した。だからこそ本書は日付ではなく Gate(完成度)で進行を管理する。

速いことは良いが、速さは順序を崩す理由にはならない。SEO編を v1.0 まで磨き、広告編はその基盤の上に建てる。各編は「診断・評価まで」の境界を守る。

迷ったら、Gateを守る。順序を崩さない。境界を越えない。
