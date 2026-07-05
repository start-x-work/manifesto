# 貢献ガイド / Contributing

このリポジトリは Marketing-OS オープンソース宣言（Manifesto）の思想ハブです。**Markdown のみで構成され、製品コードや OSS ツールのコードは含みません。** 貢献は、思想・ロードマップ・章立て・翻訳・「マーケAIの地図」への型の提案といった、文章と構造に対するものが中心になります。

This repository is the conceptual hub for the Marketing-OS Open Source Manifesto. **It is Markdown only and contains no product code or OSS tool code.** Contributions center on prose and structure: ideas, roadmap, chapters, translations, and proposals to "The Map of AI Marketing."

> ツールの実装（SEO / 広告 / SNS の CLI・Web）は各実装リポジトリで扱います。詳細は [README](./README.md) と [docs/SERVICE_STRUCTURE.md](./docs/SERVICE_STRUCTURE.md) を参照してください。
> Tool implementations (SEO / Ads / Social CLIs and web) live in their own repositories. See the [README](./README.md) and [docs/SERVICE_STRUCTURE.md](./docs/SERVICE_STRUCTURE.md).

---

## 参加のかたち / Ways to Contribute

第 6 章「参加方法」に対応する具体手順です。/ Concrete steps for Chapter 6, "How to Contribute."

- **Star** — 関心の可視化とリリース通知の受け取りに。/ Signals interest and helps with release awareness.
- **Discussions** — 思想・ロードマップ・一般的な問いの場。まずはここで方向性を共有すると議論がスムーズです。/ Best for ideas, roadmap questions, and general conversation; sharing direction here first makes larger changes smoother.
- **Issue** — 誤リンク・記述の不整合・翻訳の欠落、そして「地図への貢献」（型の追加提案・境界事例）など、トラッキングが有効なもの向け。テンプレートを用意しています。/ For broken links, inconsistencies, missing translations, and "contributing to the map" (type proposals, boundary cases)—anything that benefits from tracking. Templates are provided.
- **Pull Request** — 具体的な改善の反映に。大きな変更ほど、事前に Discussion で方向性を共有いただけると助かります。/ For concrete improvements; larger changes go smoother with an upfront Discussion.

日本語・英語のどちらでも歓迎します。言語が混在するスレッドも問題ありません。
Japanese and English are both welcome; mixed-language threads are fine.

---

## Pull Request の進め方 / How to Open a Pull Request

1. リポジトリをフォークし、作業ブランチを作成する。/ Fork the repository and create a working branch.
2. 変更を加える。**日英併記の既存様式を踏襲する**——本文を更新したら、対応する日英どちらの版も揃える。/ Make your change, **following the existing bilingual style**—when you update prose, keep both the Japanese and English versions in sync.
3. ロードマップ（第 5 章 / `master_roadmap_v3.md`）に関わる変更は、**変更理由を [CHANGELOG.md](./CHANGELOG.md) に添える**。無言の修正は透明性原則（第 4 章）に反します。/ For roadmap changes (Chapter 5 / `master_roadmap_v3.md`), **record the rationale in [CHANGELOG.md](./CHANGELOG.md)**. Silent edits violate the transparency principle (Chapter 4).
4. Pull Request を開く。テンプレート（[.github/PULL_REQUEST_TEMPLATE.md](./.github/PULL_REQUEST_TEMPLATE.md)）の項目を埋める。/ Open a Pull Request and fill in the [template](./.github/PULL_REQUEST_TEMPLATE.md).

---

## 執筆上の約束 / Writing Constraints

この宣言は公開文書です。以下は本リポジトリ固有の制約であり、Pull Request でも守ってください。
This is a public document. The following constraints are specific to this repository; please honor them in Pull Requests too.

- **固有名を出さない。** 特定のサービス名・ベンダー名・人名・固有体系名を本文に出さない。外部知見は名前を持たない「型」と「構造」に一般化して統合する。/ **No proper names.** Do not put specific service, vendor, person, or proprietary-system names in the text. Generalize outside knowledge into unnamed types and structures.
- **比較・ランキング・数値主張を避ける。** 比較表やランキング、市場規模・シェア・パーセンテージ等の数値主張は載せない。構造の記述で成立させる。/ **No comparisons, rankings, or quantitative claims.** No comparison tables, rankings, or figures such as market size, share, or percentages. Let structure carry the argument.
- **攻撃的な語彙・優劣断定を避ける。** どの型・どのツールも優劣を断定せず、リスクは構造として淡々と記述する。/ **No aggressive vocabulary or verdicts of superiority.** Assert no type or tool as superior; describe risks calmly as structure.
- **網羅カタログを新設しない。** サービス一覧のような、陳腐化と追いかけっこになる網羅リストは作らない。/ **No exhaustive catalogs.** Do not create service lists that become a losing race against staleness.

「地図への貢献」で型の追加や境界事例を報告いただいた場合も、個別のサービス名は掲載せず、名前を持たない型・構造に一般化したうえで統合します。
For "contributing to the map," proposed types and boundary cases are integrated as unnamed types and structures—individual service names are not published.

---

## ライセンス / Licensing

Pull Request を送ることで、あなたの貢献を [Apache License 2.0](./LICENSE) の下でライセンスすることに同意したものとみなします。
By submitting a Pull Request, you agree to license your contribution under the [Apache License 2.0](./LICENSE).

---

## 行動規範とセキュリティ / Code of Conduct & Security

- 参加者には [行動規範 / Code of Conduct](./CODE_OF_CONDUCT.md) の遵守をお願いします。/ All participants are expected to follow the [Code of Conduct](./CODE_OF_CONDUCT.md).
- セキュリティに関わる報告は [SECURITY.md](./SECURITY.md) の手順に従ってください。/ For security-related reports, follow [SECURITY.md](./SECURITY.md).

商用 Marketing-OS の契約・価格・個別支援については [marketing-os.jp](https://marketing-os.jp) をご利用ください。
For contracts, pricing, and tailored support for commercial Marketing-OS, see [marketing-os.jp](https://marketing-os.jp).
