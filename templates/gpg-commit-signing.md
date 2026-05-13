# GPG / SSH によるコミット署名（A-8）

GitHub の **Verified** バッジを付けるには、個人 GitHub アカウントでコミット署名を有効にする。

## SSH 署名（推奨が簡単な場合あり）

1. 既存の SSH 鍵または `ssh-keygen` で新規鍵を作成する。  
2. GitHub → **Settings → SSH and GPG keys → Signing keys** に公開鍵を登録する。  
3. ローカルで `git config --global gpg.format ssh` と `git config --global user.signingkey ~/.ssh/id_ed25519.pub`（パスは環境に合わせる）を設定する。  
4. `git config --global commit.gpgsign true` で署名付きコミットを既定にする。

## GPG 署名

1. `gpg --full-generate-key` でキーを生成する。  
2. `gpg --armor --export --export-options export-minimal <KEY_ID>` の出力を GitHub の **GPG keys** に登録する。  
3. `git config --global user.signingkey <KEY_ID>` と `git config --global commit.gpgsign true` を設定する。

詳細は GitHub 公式ドキュメント「About commit signature verification」を参照すること。
