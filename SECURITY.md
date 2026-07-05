# セキュリティ / Security Policy

このリポジトリは Manifesto の思想ハブであり、**Markdown のみで構成され、実行されるコードを含みません。** そのため典型的なソフトウェア脆弱性の対象は限定的ですが、公開文書として次のような報告を歓迎します。

This repository is the Manifesto's conceptual hub and is **Markdown only, containing no executable code.** The surface for typical software vulnerabilities is therefore limited, but as a public document we welcome the following reports.

宣言（Manifesto）第 4 章は「セキュリティ報告には誠実に応答する」と約束しています。本ポリシーはその履行手順です。
Chapter 4 of the Manifesto commits to responding conscientiously to security reports. This policy is how we honor that.

---

## 本リポジトリで受け付ける報告 / In Scope Here

- 誤解を招く／悪意あるリンク、なりすまし、フィッシングにつながる記述 / Misleading or malicious links, impersonation, or content that could enable phishing
- 商用境界を誤認させかねない記述（第 4 章「商用境界を誤認させない表現を避ける」に関わるもの） / Wording that could misrepresent the commercial boundary (relating to Chapter 4)
- 認証情報・秘密情報が誤って含まれている箇所 / Any place where credentials or secrets have been committed by mistake

## 本リポジトリの対象外 / Out of Scope Here

OSS ツール（SEO / 広告 / SNS の CLI・Web、`@start-x-work/*` パッケージ）の脆弱性は、**各実装リポジトリ**へ報告してください。それらのコードはこのリポジトリには含まれません。導線は [docs/SERVICE_STRUCTURE.md](./docs/SERVICE_STRUCTURE.md) を参照。

Vulnerabilities in the OSS tools (SEO / Ads / Social CLIs and web, the `@start-x-work/*` packages) should be reported to **their own implementation repositories**—that code does not live here. See [docs/SERVICE_STRUCTURE.md](./docs/SERVICE_STRUCTURE.md) for the map.

---

## 報告方法 / How to Report

**公開の Issue にはセキュリティ内容を書かないでください。** 次のいずれかで非公開に報告してください。

**Please do not open a public Issue for security matters.** Report privately via either of the following.

1. GitHub の **Private vulnerability reporting**（本リポジトリの「Security」タブ →「Report a vulnerability」）。/ GitHub's **Private vulnerability reporting** (the repository's "Security" tab → "Report a vulnerability").
2. リポジトリのメンテナへの非公開連絡。/ A private message to the repository maintainers.

報告には、該当箇所（ファイル・行・URL）と、想定される影響を含めてください。
Please include the location (file, line, or URL) and the potential impact.

<!--
運用メモ: 専用のセキュリティ連絡先メールアドレスを設定する場合は、ここに追記してください。
Maintainer note: if a dedicated security contact address is established, add it here.
-->

---

## 対応 / Our Response

受領した報告は速やかに確認し、妥当なものには是正で応じます。報告者のプライバシーは尊重します。商用 Marketing-OS 側の事項については、[marketing-os.jp](https://marketing-os.jp) の窓口にご案内する場合があります。

We will acknowledge reports promptly and address valid ones with a fix. The privacy of reporters is respected. For matters concerning the commercial Marketing-OS, we may direct you to the channels on [marketing-os.jp](https://marketing-os.jp).
