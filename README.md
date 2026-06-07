# The Marketing-OS Open Source Manifesto

Marketing-OS は、マーケティングの意思決定構造を業界に開くプロジェクトである。このリポジトリは、その思想と約束を言語化したハブとして機能する。

Marketing-OS is a project to open marketing’s decision-making structure to the industry. This repository is the conceptual hub where we articulate that intent and our commitments.

🔗 [marketing-os.jp](https://marketing-os.jp)

---

## 目次 / Table of Contents

1. [なぜ Start-X はオープンソース化するのか](#1-なぜ-start-x-はオープンソース化するのか) / [Why Start-X Open Sources](#1-why-start-x-open-sources)
2. [3 つの柱](#2-3つの柱) / [The Three Pillars](#2-the-three-pillars)
3. [Marketing-OS との境界線](#3-marketing-os-との境界線) / [The Marketing-OS Boundary](#3-the-marketing-os-boundary)
4. [私たちの原則](#4-私たちの原則) / [Our Principles](#4-our-principles)
5. [ロードマップ](#5-ロードマップ) / [Roadmap](#5-roadmap)
6. [参加方法](#6-参加方法) / [How to Contribute](#6-how-to-contribute)

関連文書: [editor.md](./editor.md)（編集者性と OSS の接続）・[licenses.md](./licenses.md)（ライセンス選定の理由）・[master_roadmap_v3.md](./master_roadmap_v3.md)（OSS 全体ロードマップ）

---

## 1. なぜ Start-X はオープンソース化するのか

マーケティング業界には、長く続いてきた分業と委託の構造がある。代理店は実行力と専門性を提供し、クライアントは成果と説明責任を求める。このモデルは一定の条件下では有効だが、データと AI が意思決定の速度を上げる現在、次の歪みが目立ちやすくなる。第一に、戦略と実行の境界が曖昧になり、意思決定の所在が分散する。第二に、現場の知見がブラックボックス化し、組織内で構造として再利用されにくい。第三に、ツールとプロセスがベンダー単位に閉じ、業界全体の学習曲線が平坦化しにくい。

Start-X がオープンソース化を選ぶ理由は、対立を煽るためではない。編集可能な「素材」としてコードと判断の枠組みを外に出し、誰もが検証・改良・接続できる状態に近づけるためである。マーケティングの本質は、素材を組み替え、文脈を編集し、意思決定の場を整える行為に近い。OSS はその行為を、単一組織の内製だけでなく、業界横断の集合的な編集へと拡張するための器である。

私たちは、完成度の高い商用プロダクト（Marketing-OS）を継続して開発する。同時に、診断やツールの一部、思想の言語化をオープンにすることで、利用者が自らの文脈に合わせて組み替えられる余地を確保する。これは「すべてを無償で渡す」ことではない。境界線は第 3 章で明示する。ここで述べるのは、意思決定の構造を閉じたまま独占しないという姿勢である。

個人の編集者性から組織の編集者性へ、さらにコミュニティの編集者性へと接続を広げる。それが Start-X における OSS の意味である。詳細な思想的接続は [editor.md](./editor.md) に委ねる。

## 1. Why Start-X Open Sources

Marketing has long relied on specialization and delegation. Agencies supply execution and expertise; clients demand outcomes and accountability. That arrangement still works in many contexts, but as data and AI accelerate decisions, three strains appear. First, the line between strategy and execution blurs, scattering ownership of decisions. Second, hard-won insight is trapped in black boxes, resisting reuse as durable structure inside organizations. Third, tools and processes stay vendor-shaped, which makes it harder for the industry as a whole to climb a shared learning curve.

Start-X chooses open source not to amplify conflict. We publish code and decision scaffolds as editable material so others can verify, improve, and connect them. Marketing, at its core, resembles editing: rearranging raw material, reshaping context, and convening a decision-making field. Open source extends that act beyond a single organization toward collective editing across the industry.

We continue to build a serious commercial product—Marketing-OS—while opening parts of diagnostics, tooling, and conceptual writing so adopters can rearrange them to fit their context. This is not “give everything away for free.” Chapter 3 states the boundary clearly. What we affirm here is a stance against hoarding decision structure in silence.

We extend the chain from individual editorial practice to organizational practice, and onward to community practice. That is what open source means for Start-X. For the deeper philosophical bridge, see [editor.md](./editor.md).

---

## 2. 3 つの柱

Marketing-OS が扱う業務領域は広いが、OSS としてまず体系化するのは次の三本柱である。SEO、広告（Ads）、ソーシャル（Social）である。いずれも、チャネル固有の戦術だけでなく、意思決定に耐える構造（計測の置き方、優先順位の付け方、再現性の持たせ方）を扱う。

SEO 編は、検索エンジン最適化に加え、LLMO／AEO のように生成 AI 経由の可視性を問う領域を含む。最初の実装集中先であり、CLI から段階的に機能を積み上げる。広告編は、運用の自動化だけでなく、配信判断と学習ログの構造化を志向する。着手はロードマップ上で後ろに置き、SEO 編で得た基盤を再利用する。SNS 編は、投稿スケジュールの効率化に留まらず、コミュニティの声を意思決定に接続する支援を目指す。順序は SEO 先行、次に広告、次に SNS である。理由は単純で、オーガニック側の診断と資産設計が、他チャネルの前提になりやすいからである。

各柱の詳細な約束と除外事項は、カテゴリ別 README に記す。

- [SEO 編](./seo/README.md)
- [広告編](./ads/README.md)
- [SNS 編](./social/README.md)

## 2. The Three Pillars

Marketing-OS spans a wide surface area, but we first systematize three pillars in open source: SEO, Ads, and Social. Each addresses channel-specific work while foregrounding durable structure—where to measure, how to prioritize, and how to make decisions reproducible.

The SEO pillar includes classic search optimization and visibility through generative interfaces (LLMO / AEO). It is our first implementation focus, growing from a CLI outward. The Ads pillar targets structured learning logs and delivery judgment, not automation alone; it follows SEO so we can reuse foundations. The Social pillar aims to connect community signal to decisions, not merely to schedule posts. The sequence is SEO, then Ads, then Social, because organic diagnosis and asset design tend to precondition other channels.

For detailed commitments and explicit non-goals, read each category README: [SEO](./seo/README.md), [Ads](./ads/README.md), [Social](./social/README.md).

---

## 3. Marketing-OS との境界線

OSS で提供するものは、再利用可能な部品、診断、テンプレート、CLI／ライブラリ、そして本 Manifesto に代表される思想の言語化である。利用者はフォークや組み込みを通じて、自社のスタックやプロセスに接続できる。

商用の Marketing-OS で提供するものは、意思決定の OS としての統合体験である。組織横断のワークフロー、AI CMO のような伴走インターフェース、運用 BPO、契約と SLA に裏打ちされたサポートがここに含まれる。つまり、OSS は「素材と型」、商用は「運用と責任の束ね方」という住み分けである。

この線引きは二つの価値を同時に守る。第一に、競合が商用成果物を無条件に取り込んで再販する余地を狭め、持続可能な開発投資を守る。第二に、利用者がベンダーロックなき入り口を持てるようにする。両方を使う場合のメリットは明確である。OSS で早期検証と内製接続を行い、商用で組織規模の運用と責任分界を引き受ける。逆に、OSS のみで十分なチームもある。その選択を尊重する。

境界は固定ではない。ロードマップと合意形成に基づき、OSS 側へ下ろすもの・商用側に残すものは見直す。ただし、見直しのたびに理由を公開する。

## 3. The Marketing-OS Boundary

Open source supplies reusable parts, diagnostics, templates, CLIs and libraries, and conceptual writing like this Manifesto. Adopters can fork or embed and connect to their own stack and process.

Commercial Marketing-OS supplies the integrated experience of a decision OS: cross-functional workflows, interfaces such as AI CMO, BPO operations, and support backed by contracts and SLAs. In short, open source offers material and patterns; commercial offers how operations and accountability are bundled.

This boundary protects two values at once. First, it narrows the path for competitors to lift commercial outcomes wholesale for resale, preserving sustainable investment. Second, it preserves a vendor-lock-in-free entry for adopters. Using both is straightforward: validate and connect internally with OSS, then scale operations and responsibility with commercial services. Some teams will thrive on OSS alone—we respect that.

The boundary is not frozen. As the roadmap and community consensus evolve, we may move features between OSS and commercial. Whenever we do, we will publish the rationale.

---

## 4. 私たちの原則

品質について。Coming Soon は明示し、未実装を曖昧に見せない。ドキュメントとコードの両方で、到達度合いを正直に示す。

コミュニティについて。議論は編集的であるべきだ。素材の再配置、前提の言語化、代替案の提示を歓迎する。人格攻撃や恫喝は許容しない。

責任について。セキュリティ報告には誠実に応答する。ライセンス条件を尊重し、商用境界を誤認させない表現を避ける。利用者が安全に試せる導線を維持する。

透明性について。ロードマップは公開し、変更理由を可能な限り説明する。成功指標と撤退の目安については、運用ドキュメント（メトリクスと判断ライン）に委ね、定期的にレビューする。

## 4. Our Principles

Quality: label Coming Soon clearly; never imply readiness we have not earned. Be honest in both docs and code about what is shipped.

Community: debate should be editorial—rearranging material, stating assumptions, offering alternatives. Personal attacks and intimidation are not acceptable.

Responsibility: respond conscientiously to security reports, honor license terms, and avoid messaging that misstates the commercial boundary. Keep paths safe for experimentation.

Transparency: publish the roadmap and explain changes whenever feasible. For success metrics and retreat lines, we rely on operational documentation and review them on a fixed cadence.

---

## 5. ロードマップ

粗い位相は [Start-X OSS マスターロードマップ v3.0](./master_roadmap_v3.md) で管理する。Phase 1 は完了、Phase 2 は実質完了、Phase 3 は SEO CLI のコード完了・公開作業中である。以降は Phase 4（SEO Web UI）、Phase 5（広告準備 + SEO v1.0）、Phase 6（広告 v0.1）へ進む。

月固定のスケジュールではなく、Gate（完成度）で進行を判断する。優先順位が入れ替わる場合は、ロードマップ上で理由を明示する。

成果と撤退に関する判断基準は、別紙の運用基準に記す。ここでは、定期的に読み直し、必要なら Manifesto 本文を微修正するプロセスだけを約束する。

## 5. Roadmap

The current sequence is tracked in the [Start-X OSS Master Roadmap v3.0](./master_roadmap_v3.md). Phase 1 is complete, Phase 2 is effectively complete, and Phase 3 has completed the SEO CLI code while publication work remains. Next come Phase 4 (SEO Web UI), Phase 5 (Ads preparation + SEO v1.0), and Phase 6 (Ads v0.1).

We now track progress by gates rather than fixed monthly dates. If priorities shift, we will state the reason on the roadmap itself.

Success and retreat criteria live in operational documentation. Here we only commit to periodic review and small Manifesto edits when feedback warrants them.

---

## 6. 参加方法

参加は次の四種類が主である。**Star** は関心の可視化とリリース通知の助けになる。**Discussions** は思想・ロードマップ・一般的な問いの場として推奨する。**Issue** はバグや機能要望など、トラッキングが有効なもの向けである（リポジトリごとにテンプレートあり）。**Pull Request** は改善案の具現化に使ってほしい。大きな変更ほど、事前に Discussion で方向性を共有いただけるとスムーズです。

日本語と英語のどちらでも歓迎します。言語が混在するスレッドも問題ありません。行動規範は Organization の Contributor Covenant（各リポの `.github` 経由で参照）に従います。

商用 Marketing-OS の契約・価格・個別支援については、[marketing-os.jp](https://marketing-os.jp) 側の導線をご利用ください。

## 6. How to Contribute

Four primary paths: **Stars** signal interest and help with release awareness. **Discussions** are best for ideas, roadmap questions, and general conversation. **Issues** suit bugs and feature requests that benefit from tracking (templates vary by repository). **Pull Requests** carry concrete improvements—larger changes go smoother with an upfront Discussion.

Japanese and English are both welcome; mixed-language threads are fine. We follow the organization’s Contributor Covenant (via each repo’s `.github` links).

For contracts, pricing, and tailored support for commercial Marketing-OS, please use the paths published on [marketing-os.jp](https://marketing-os.jp).

---

## Phase 1 運用メモ（指示書 v2.0）

- **実施記録・チェックリスト:** [templates/setup-history-v2.md](./templates/setup-history-v2.md)  
- **付録A（事前判断の記録）:** [templates/appendix-a-decisions.md](./templates/appendix-a-decisions.md)  
- **付録D（メトリクス記録）:** [templates/metrics.md](./templates/metrics.md)  
- **付録E（公開アナウンス下書き）:** [templates/announcement-drafts/](./templates/announcement-drafts/)  
- **コミット署名（付録A-8）:** [templates/gpg-commit-signing.md](./templates/gpg-commit-signing.md)

🔗 [marketing-os.jp](https://marketing-os.jp) · Organization: [start-x-work](https://github.com/start-x-work)
