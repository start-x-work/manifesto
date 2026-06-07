# Start-X OSS 実行手順書 — 環境準備 → N0(SEO CLI公開)

**対象:** 山口偉大(macOS 想定)
**目的:** ゼロ状態から `@start-x-work/mos-seo` を npm + GitHub Release で公開するまで、ターミナルとCursorにそのまま貼って実行できる手順。
**使い方:** 上から順に。各STEPの「✅ 確認」が緑になってから次へ。`【ターミナル】`= ターミナルに貼る / `【Cursor】`= Cursor の Agent(Composer)に貼る。

---

## 全体像

```
PART 1  環境準備(STEP 0〜8)      … 1回だけ
PART 2  N0 実装(SEO CLI仕上げ)    … Cursorで実行
PART 3  N0 公開(npm + Release)    … ターミナルで実行
PART 4  並走タスク N3(広告調査)    … 別タブで先行
PART 5  完了報告フォーマット
```

---

# PART 1 — 環境準備

## STEP 0 — 現状確認

`【ターミナル】`
```bash
node --version; git --version; gh --version; pnpm --version
```

各行に数字が出れば導入済み。`command not found` のものだけ、対応するSTEPを実行する。全部入っていれば STEP 5 へ飛んでよい。

---

## STEP 1 — Homebrew(未導入なら)

`【ターミナル】`
```bash
which brew || /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

インストール後、表示される「Next steps」の `eval "$(...)"` 行を必ず実行(PATHを通す)。

✅ 確認
```bash
brew --version
```

---

## STEP 2 — Node.js 22 LTS(nvm経由)

`【ターミナル】`
```bash
brew install nvm
mkdir -p ~/.nvm
```

`【ターミナル】`(nvmをshellに登録。zsh前提)
```bash
cat >> ~/.zshrc <<'EOF'

export NVM_DIR="$HOME/.nvm"
[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"
EOF
source ~/.zshrc
```

`【ターミナル】`
```bash
nvm install 22
nvm use 22
nvm alias default 22
```

✅ 確認
```bash
node --version    # v22.x.x
```

---

## STEP 3 — pnpm(Corepack経由)

`【ターミナル】`
```bash
corepack enable
corepack prepare pnpm@latest --activate
```

✅ 確認
```bash
pnpm --version
```

---

## STEP 4 — GitHub CLI + 認証

`【ターミナル】`
```bash
brew install gh
gh auth login
```

対話で以下を選択:
- `GitHub.com`
- `HTTPS`
- `Authenticate Git with your GitHub credentials` → Yes
- `Login with a web browser`(表示されたコードをブラウザに入力)

✅ 確認
```bash
gh auth status    # "Logged in to github.com as ..." が出る
```

---

## STEP 5 — Git 初期設定(初回のみ)

`【ターミナル】`(名前・メールは自分のものに置換)
```bash
git config --global user.name "Takehiro Yamaguchi"
git config --global user.email "takehiro.yamaguchi@start-x.work"
```

✅ 確認
```bash
git config --global user.name; git config --global user.email
```

---

## STEP 6 — リポジトリをクローン

`【ターミナル】`
```bash
mkdir -p ~/projects && cd ~/projects
gh repo clone start-x-work/marketing-os-seo
cd marketing-os-seo
```

✅ 確認
```bash
ls    # packages/ docs/ package.json LICENSE NOTICE などが見える
```

---

## STEP 7 — 依存インストール & 動作確認(最初の関門)

`【ターミナル】`
```bash
pnpm install
pnpm build
pnpm test
```

✅ 確認:エラーなく完了すること。
- `pnpm test` が緑 → 既存コードは健全。PART 2 へ。
- 赤(失敗)が出たら → **出力をそのまま Claude に貼る**。PART 2 の前に直す。

---

## STEP 8 — Cursor セットアップ

1. https://cursor.com からインストール
2. Cursor を起動 → `File > Open Folder` → `~/projects/marketing-os-seo` を開く
3. 指示書を参照可能にする:Claude が作った以下のファイルを、リポ内 `docs/` にコピー
   - `phase3_seo_cli_finalize_v1.md`
   - `implementation_blueprint_v2.md`(全体地図)

`【ターミナル】`(ダウンロード済みファイルを docs/ に置く例。パスは実際の場所に置換)
```bash
cp ~/Downloads/phase3_seo_cli_finalize_v1.md ~/projects/marketing-os-seo/docs/
cp ~/Downloads/implementation_blueprint_v2.md ~/projects/marketing-os-seo/docs/
```

✅ 確認:Cursorのファイルツリーに `docs/phase3_seo_cli_finalize_v1.md` が見える。

---

### PART 1 ゴール

```
✓ node v22 / pnpm / gh / git が動く
✓ gh 認証済み
✓ marketing-os-seo をクローン
✓ pnpm install && build && test が緑
✓ Cursor でフォルダを開き、docs/ に指示書を配置
```

ここまで来たら PART 2 へ。

---

# PART 2 — N0 実装(SEO CLI 仕上げ)

Cursor の Agent / Composer モードを開き(`Cmd+I`)、以下をそのまま貼る。

`【Cursor】`
```
@docs/phase3_seo_cli_finalize_v1.md に従って marketing-os-seo を Gate B(リリース品質)まで仕上げます。

まず F1(現状コードの監査)を実行してください:
- packages/core と packages/cli の既存実装を読む
- 4機能(LLMO診断/サイト診断/コンテンツブリーフ/キーワード)の実装状況・不足・リスクを一覧化
- この段階ではコードを変更せず、監査レポートだけ出す

監査後、F2 → F3 → F4 → F5 の順で補強:
- F2: 入力検証(zod)・エラーハンドリング(CliError/FetchError、15秒タイムアウト)
- F3: テスト強化(vitest。AIはモックして実APIキー不要で通るように)
- F4: CI(.github/workflows/ci.yml:pnpm install→lint→build→test、Node22)
- F5: README/docs/USAGE.md の整備

必須ルール:
- 既存の動くコードは尊重し、不足を補う(大規模リファクタはしない)
- AI は必ず provider 経由(Gemini直叩き禁止)
- 本文の自動生成・自動投稿は実装しない(診断・ブリーフまで)
- 全コマンドに --format json を用意
- .env は作らず .env.example のみ。APIキーは私が手動設定する

各タスク完了ごとに git commit してください。
F6(npm公開)・F7(アナウンス)はまだ実行しないでください。私が PART 3 で行います。
判断に迷う箇所(特に core を別パッケージ公開するか cli にバンドルするか)は私に確認してください。
```

### 進め方

- Agent が監査レポートを返したら、内容を確認して「F2から進めて」と返す。
- 各段階で差分を確認し、`pnpm test` が緑か都度チェック。
- F5 まで終わったら、ローカルにコミットが積まれた状態になる。

✅ 確認(ターミナルで)
```bash
cd ~/projects/marketing-os-seo
pnpm lint && pnpm build && pnpm test    # 緑
git log --oneline -10                    # F2〜F5のコミットが積まれている
```

---

# PART 3 — N0 公開(npm + Release)

ここから**山口さん自身がターミナルで実行**(Cursorではない)。

## STEP 3-1 — npm アカウント準備(初回のみ)

1. https://www.npmjs.com でアカウント作成(未作成なら)
2. npm上に organization `start-x-work` を作成
   - npmサイト右上 → Add Organization → 名前 `start-x-work`(無料のPublicでよい)
   - ※ これは npm の scope。GitHub org とは別管理

`【ターミナル】`
```bash
npm login    # ブラウザ認証
npm whoami    # ユーザー名が出ればOK
```

## STEP 3-2 — 公開前の最終確認

`【ターミナル】`
```bash
cd ~/projects/marketing-os-seo
git push origin main                      # PART 2 のコミットを push
pnpm install --frozen-lockfile
pnpm lint && pnpm build && pnpm test       # 全部緑を確認
```

## STEP 3-3 — cli の package.json を確認

`【Cursor】` または手動で `packages/cli/package.json` を開き、以下があるか確認(無ければ追記):
```jsonc
{
  "name": "@start-x-work/mos-seo",
  "version": "0.1.0",
  "bin": { "mos-seo": "./dist/index.js" },
  "files": ["dist"],
  "license": "Apache-2.0",
  "publishConfig": { "access": "public" }
}
```

`tsup.config.ts` に core バンドル設定があるか(無ければ Cursor に追加依頼):
```typescript
noExternal: [/@start-x-work\/.*/]
```

## STEP 3-4 — npm 公開

`【ターミナル】`
```bash
cd ~/projects/marketing-os-seo/packages/cli
npm publish
```

✅ 確認
```bash
npm view @start-x-work/mos-seo version    # 0.1.0 が返る
```

## STEP 3-5 — タグ & GitHub Release

`【ターミナル】`
```bash
cd ~/projects/marketing-os-seo
git tag v0.1.0
git push origin v0.1.0
gh release create v0.1.0 \
  --repo start-x-work/marketing-os-seo \
  --title "SEO Toolkit v0.1.0 (CLI)" \
  --notes "First public release. LLMO/AEO audit, technical SEO audit, content brief generator, keyword intent mapper. CLI only. See docs/USAGE.md."
```

## STEP 3-6 — 外部動作確認(最重要)

`【ターミナル】`(リポ外の任意の場所で)
```bash
cd ~
npx @start-x-work/mos-seo audit site https://example.com --format json
```

✅ 確認:JSON で診断結果が返る → **N0 完了**。

---

# PART 4 — 並走タスク N3(広告API調査)

N3 はコード不要・N0 と無関係なので、**PART 2 と同時に別タブで進められる**(待ち時間ゼロ化)。Cursor で新しいチャットを開き、以下を貼る。

`【Cursor】`
```
@docs/implementation_blueprint_v2.md の N3 に従い、広告API調査を行います。
marketing-os-ads の着手準備として、各広告プラットフォーム
(Google Ads / Meta Ads / Yahoo!広告 / LINE Ads / X Ads / TikTok Ads)について、
公式APIドキュメントを調べ docs/api-research.md にまとめてください:
- 認証方式
- 主要な「読み取り」エンドポイント(キャンペーン一覧・実績取得)
- レート制限
- 自動化・サードパーティツールに関する規約条項

着手プラットフォームの確定とスコープ判断・規約の法的判断は私が行うので、
判断材料の比較表まで作ってください。あなたは事実の調査・整理まで。
```

※ これは marketing-os-seo とは別リポの話だが、調査メモなのでどこで作ってもよい(後で ads リポに移す)。

---

# PART 5 — 完了報告フォーマット

N0 が終わったら、Claude に以下を貼って次ノードの指示を受ける:

```
N0 完了。
- npm: https://www.npmjs.com/package/@start-x-work/mos-seo
- Release: https://github.com/start-x-work/marketing-os-seo/releases/tag/v0.1.0
- npx 動作確認: OK
次は何を着手すればいい?
```

Claude 側で確認後、次の最短経路を指示する:
- **N1(SEO v1.0)** に着手 → `phase5_ads_prep_seo_v1.md` 系統A
- **N2(SEO Web)** を並走 → `phase4_seo_web_impl_v1.md`
- (N3 が終わっていれば、後の N5 が待ちなしで入れる)

---

# トラブル時の貼り方

詰まったら、**実行したコマンドとエラー出力をそのまま** Claude に貼る。例:

```
STEP 7 で pnpm test が失敗。出力:
<ここにエラー全文>
```

エラー全文があれば原因を特定して直す。推測で進めず、出力を見せるのが最短。

---

# このドキュメントの位置づけ

```
implementation_blueprint_v2.md    全体地図(N0〜N10、依存・並列)
remaining_nodes_impl_v1.md        N4/N7/N8/N9/N10 詳細
phase3〜7_*.md                     各ノード詳細
▶ 本書(実行手順書)               環境構築 → N0 を「手を動かす順」に翻訳
```

他は「何を作るか」の設計図。本書は「いまターミナルに何を打つか」の操作手順。N1以降に進むときは、同じ要領で各 phase 指示書を Cursor に渡せばよい。

---

**次の一手:** PART 1 の STEP 0 を実行し、結果を Claude に貼る。足りないツールだけに手順を絞る。
