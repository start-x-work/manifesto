# Git運用ルール（技術的強制なし・プロセス徹底型）

GitHub Rulesets / Branch protection をプランの制約で使えない（または意図的に使わない）プロジェクト向けの、**運用ルールで Git Flow を守るための指示書**。人間の開発者・AIエージェント（Claude Code 等）の両方が、このドキュメントをそのまま行動規範として使えるように書いている。

他プロジェクトに導入する際は、そのまま `CLAUDE.md` / `AGENTS.md` / `CONTRIBUTING.md` に貼り付けて使う。

---

## 0. 前提

- `main` / `develop` に対する **push 保護が GitHub 側で技術的に効いていない**（Free/Personal プランの private repo 等）。
- そのため、ここに書くルールは **性善説ではなく「破ったら誰でも気づける仕組み」** として設計する（後述のセルフチェック・PR 経由の徹底）。
- 例外を作らない。オーナー自身・AIエージェントも含め、全員がこのルールに従う。

---

## 1. 絶対原則

1. **`main` と `develop` に直接 `git push` しない。** 例外なし。
2. **すべての変更は PR 経由でのみ `main` / `develop` に入る。**
3. **PR をマージする前に、必ず CI を自分の手元（または CI 環境）で独立して通す。** PR の説明文に書かれた「テスト済み」「CI 通過」を鵜呑みにしない — 特に他者・他ツール（Cursor、Codex 等の別 AI エージェント含む）が作成した PR は、その主張を検証してから信じる。
4. **squash-merge されたブランチは、そのローカル/リモートブランチのコミット履歴を再利用しない。** 次の作業は必ず最新の `main`/`develop` から分岐し直す（`git checkout -B <branch> origin/<base>`）。同じブランチ名の使い回しは可だが、**ポインタは毎回最新の base にリセットする**。
5. **force push は `--force-with-lease` のみ許可。** 素の `--force` は使わない（他者の変更を握りつぶす事故を防ぐ）。

---

## 2. ブランチ構成

| ブランチ | 役割 | 分岐元 | マージ先 |
|---|---|---|---|
| `main` | 本番。常にリリース可能な状態 | — | — |
| `develop` | 開発統合ブランチ | `main` | `main`（`release/*` 経由） |
| `feature/*` | 個々の機能・修正 | `develop` | `develop` |
| `release/*` | リリース前の最終調整 | `develop` | `main` と `develop` の両方 |
| `hotfix/*` | 本番の緊急修正 | `main` | `main` と `develop` の両方 |

命名規則: `feature/<番号-または-連番>-<内容>`。番号がなければ日付や連番でも可（例: `feature/2026-08-18-fix-mail-status`）。

---

## 3. PR 作成〜マージのチェックリスト（毎回必須）

- [ ] 1. ブランチを作業対象の base（main or develop）の最新から作り直したか
  ```bash
  git fetch origin <base> -q && git checkout -B <branch> origin/<base>
  ```
- [ ] 2. 変更後、プロジェクトの CI コマンドをローカルで実行し green を確認したか
  （例: `npm run ci` / `make test` 等。プロジェクトの README/package.json で確認）
- [ ] 3. 他者・他ツール作成の PR をマージする場合、そのPRのブランチを fetch して
  "自分の手元でも" CI を回して独立検証したか（PR 本文の主張を鵜呑みにしない）
- [ ] 4. コミットメッセージ・PR 説明に、変更の背景（why）が書かれているか
- [ ] 5. draft PR として作成 → デプロイ/CI のステータスが green になるまで待つ →
  ready for review にしてからマージ（squash 推奨、履歴を汚さない）
- [ ] 6. マージ後、その PR の元ブランチをリモートから削除する
  ```bash
  git push origin --delete <branch>
  ```

---

## 4. 複数 PR が同じファイルを編集していて競合する場合

squash-merge は新しいコミット SHA を作るため、`git branch --merged` では検出できない。以下の手順で統合する。

```bash
# 1. 最新の base から統合用ブランチを作る
git fetch origin main -q
git checkout -B integrate/<topic> origin/main

# 2. 各PRのブランチを取得して順に cherry-pick
git fetch origin <pr-branch-1>:<pr-branch-1> -q
git cherry-pick <commit-sha-1>
git fetch origin <pr-branch-2>:<pr-branch-2> -q
git cherry-pick <commit-sha-2>
# コンフリクトが出たら、両方の変更意図を汲んで手動解消（片方を機械的に勝たせない）

# 3. 統合後、CI を通す
npm run ci   # プロジェクトの CI コマンド

# 4. push → PR作成 → マージ
git push --force-with-lease -u origin integrate/<topic>

# 5. 統合元の個別 PR は「このPRに統合してクローズした」旨をコメントし、close する
#    （マージはしない。重複コミットを本線に残さないため）
```

---

## 5. AIエージェント（Claude Code 等）向けの追加ルール

- 他の AI/ボット（Cursor、Codex 等）が作成した PR も、内容は必ず自分で読み、diff を精査してから扱う。「セキュリティ修正」「Critical Bug」等と書かれていても、それだけでは信用しない — ロジックが正しいか、既存のコードパターンと整合しているかを確認する。
- 危険度の高い変更（認可・決済・個人情報系）ほど、マージ前の独立検証を厚くする。型チェック・ビルドが通るだけでなく、意図した通りの分岐になっているかをコード上で追う。
- ブランチ削除・force push など破壊的操作の前には、必ず現在の状態（`git status` / `git log`）を確認する。
- 「デプロイ完了しましたか？」と聞かれたら、正直に確認できる範囲を答える。CI の green ＝コードが正しいことの確認であり、本番デプロイの完了確認ではない（別物として扱う）。自分でアクセスできないドメインがあれば、その旨を正直に伝えてユーザーに確認を依頼する。
- 同じ種類の PR が複数（例: 同じ検証スクリプトファイルを別 PR が新規作成）ある場合は、機械的に順にマージせず、先に全件を読んで統合方針を立ててから着手する。

---

## 6. 定期メンテナンス

- 月1回程度、`git branch -r` でリモートブランチを棚卸しし、マージ済み・クローズ済みで不要なものを削除する（本ドキュメント §3-6 のとおり、本来はマージ時点で都度削除するのが理想）。
- 判定基準: その PR が `merged: true` または「他 PR に統合済みとして close」されているか（GitHub PR 一覧 API の `list` レスポンスの `merged` フィールドは不正確な場合があるため、疑わしい場合は個別に `get` して確認する）。
