# The Marketing-OS Open Source Manifesto

Marketing-OS は、マーケティングの意思決定構造を業界に開くプロジェクトである。このリポジトリは、その思想と約束を言語化したハブとして機能する。

Marketing-OS is a project to open marketing’s decision-making structure to the industry. This repository is the conceptual hub where we articulate that intent and our commitments.

🔗 [marketing-os.jp](https://marketing-os.jp)

---

## 目次 / Table of Contents

1. [なぜ Start-X はオープンソース化するのか](#1-なぜ-start-x-はオープンソース化するのか) / [Why Start-X Open Sources](#1-why-start-x-open-sources)
2. [3 つの柱](#2-3つの柱) / [The Three Pillars](#2-the-three-pillars)
   - 2.5. [マーケAIの地図](#25-マーケaiの地図) / [The Map of AI Marketing](#25-the-map-of-ai-marketing)
3. [Marketing-OS との境界線](#3-marketing-os-との境界線) / [The Marketing-OS Boundary](#3-the-marketing-os-boundary)
4. [私たちの原則](#4-私たちの原則) / [Our Principles](#4-our-principles)
5. [ロードマップ](#5-ロードマップ) / [Roadmap](#5-roadmap)
6. [参加方法](#6-参加方法) / [How to Contribute](#6-how-to-contribute)

関連文書: [editor.md](./editor.md)（編集者性と OSS の接続）・[licenses.md](./licenses.md)（ライセンス選定の理由）・[master_roadmap_v3.md](./master_roadmap_v3.md)（OSS 全体ロードマップ）・[implementation_blueprint_v2.md](./implementation_blueprint_v2.md)（実装ブループリント）・[next_phase_manifesto_sync_n1_v1.md](./next_phase_manifesto_sync_n1_v1.md)（N1 フェーズ）・[next_phase_manifesto_sync_n2_v1.md](./next_phase_manifesto_sync_n2_v1.md)（運用完成度フェーズ）・**[docs/QUICKSTART.md](./docs/QUICKSTART.md)**（利用者向けクイックスタート）・**[docs/SERVICE_STRUCTURE.md](./docs/SERVICE_STRUCTURE.md)**（現行 OSS サービス構成）

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

SEO 編は、検索エンジン最適化に加え、LLMO／AEO のように生成 AI 経由の可視性を問う領域を含む。**v0.1 は CLI + Web UI として公開済み**（2026年6月）。広告編・SNS 編も同様に v0.1 を公開済み。三本柱は `@start-x-work/mos-kit` 共通基盤の上に、診断・評価・構造化に限定して提供する。統合入口は `@start-x-work/marketing-os`（npm）と [docs/QUICKSTART.md](./docs/QUICKSTART.md)。

各柱の公開形態（CLI / Web / npm）は [docs/SERVICE_STRUCTURE.md](./docs/SERVICE_STRUCTURE.md) を参照。

各柱の詳細な約束と除外事項は、カテゴリ別 README に記す。

- [SEO 編](./seo/README.md)
- [広告編](./ads/README.md)
- [SNS 編](./social/README.md)

## 2. The Three Pillars

Marketing-OS spans a wide surface area, but we first systematize three pillars in open source: SEO, Ads, and Social. Each addresses channel-specific work while foregrounding durable structure—where to measure, how to prioritize, and how to make decisions reproducible.

The SEO pillar includes classic search optimization and visibility through generative interfaces (LLMO / AEO). **v0.1 is live** as CLI + Web UI (June 2026). Ads and Social pillars are also published at v0.1. All three share `@start-x-work/mos-kit` and stay within diagnosis, evaluation, and structure—not automation. Unified entry: `@start-x-work/marketing-os` on npm and [docs/QUICKSTART.md](./docs/QUICKSTART.md).

See [docs/SERVICE_STRUCTURE.md](./docs/SERVICE_STRUCTURE.md) for the current OSS service catalog.

For detailed commitments and explicit non-goals, read each category README: [SEO](./seo/README.md), [Ads](./ads/README.md), [Social](./social/README.md).

---

## 2.5 マーケAIの地図

### 2.5.1 なぜ地図か

この章で言う「地図」とは、マーケティングに関わる AI ツール群を、固有のサービス名ではなく「何を代行し、何を手元に残すか」という構造で分類した見取り図のことである。

いま、AI を掲げるマーケティングツールは急速に増えている。投稿や配信を自動化すると謳うもの、広告運用を最適化すると謳うもの、クリエイティブを量産するもの、成果を可視化するもの。名前を追いかけるだけの理解は、名前の入れ替わりとともに古びる。しかし構造は名前より長く生きる。だから私たちは、名前のカタログではなく構造の地図を描く。

ツールが増えるほど、選定の議論は「どれが良いか」に流れやすい。だが本当に問うべきは「そのツールは意思決定の何を代行し、何を自分たちの手元に残すのか」である。この問いを持たないまま導入を重ねると、施策は動いているのに「なぜそう決めたのか」を誰も説明できない状態——私たちはこれを判断の濁りと呼ぶ——が静かに進行する。地図を持つ目的は、ツールを格付けすることではない。自分たちのスタックのどこに判断が残り、どこで消えているかを見えるようにすることである。

なお、この地図は特定のサービスを評価も推薦もしない。挙げるのは型と構造だけであり、それぞれの型に優劣はない。

### 2.5.2 四つの型

分類の軸は一つだけである——そのツールは、マーケティングという営みのどの部分を代行するのか。操作を代行するのか、制作を代行するのか、把握を代行するのか、それとも判断の記録と照合を支えるのか。この軸で見ると、マーケティング AI ツールは、おおむね次の四つの型に分類できる。多くのサービスは複数の型を兼ねるが、中心となる重心は必ずどれかにある。自分の使っているツールがどれに当たるか迷ったら、「このツールが止まったとき、止まるのは何か」を考えるとよい。配信が止まるなら実行型、制作が止まるなら生成型、把握が止まるなら観測型、判断の記録が止まるなら判断構造化型である。

**実行型。** 実行型とは、投稿・入札・配信・入稿といった操作そのものを、人に代わって自動で行うツールの型である。構造としては、目標値を与えると、個々の操作判断（いつ・どこに・いくらで）をアルゴリズムが引き受ける。得意なのは、人手では追いつかない頻度と規模の操作を安定してこなすことだ。固有のリスクは、操作の判断根拠が外から見えにくくなること——ブラックボックス化——であり、結果として「なぜその配分になったのか」を組織が説明できなくなる判断の濁りが起きやすい。これは型の欠陥というより、実行を委ねるという構造が必然的に伴うトレードオフである。

**生成型。** 生成型とは、コピー・画像・動画などのクリエイティブを AI が作り出すツールの型である。構造としては、指示や過去の素材を入力に、制作物を出力する。得意なのは、制作の初速と物量の確保である。固有のリスクは均質化——同じ基盤技術から似た出力が生まれるため、市場全体で表現が収斂しやすい。その結果、差別化の源泉は「何を作れるか」から「何を選び、なぜそれを選ぶか」へ移動する。生成型を使いこなすほど、選定の判断こそが希少資源になる。

**観測型。** 観測型とは、計測・帰属・可視化——つまり「何が起きたか」を捉えることを担うツールの型である。構造としては、行動データを収集し、指標として整えて提示する。得意なのは、事実の把握を人の記憶や感覚から切り離すことだ。固有のリスクは、数字は残るのに判断が残らないこと。ダッシュボードは「何が起きたか」を教えてくれるが、「それを見て何を決めたか」「なぜそう決めたか」は別の場所——多くの場合、誰かの頭の中——に消えていく。

**判断構造化型。** 判断構造化型とは、意思決定そのものとその根拠を記録・構造化し、後から照合できるようにするツールの型である。構造としては、診断・評価・優先順位付けの過程を、再利用可能な形式で蓄積する。得意なのは、判断を個人の暗黙知から組織の資産へ移すことだ。固有のリスクは、記録という行為自体に規律が要ること——書かれない判断は構造化されない——であり、運用の習慣が伴わなければ空の器になる。

繰り返すが、四つの型に優劣はない。それぞれが異なる仕事を引き受けており、異なるものを代行し、異なるものを手元に残す。

### 2.5.3 型の組み合わせ

実務では、単一の型だけでツール構成が完結することはまずない。生成型で素材を作り、実行型で配信し、観測型で計測する——この組み合わせ自体は自然であり、否定すべきものではない。物量と速度の面では、これで多くの業務が回る。

問題は、組み合わせの継ぎ目で何かが失われやすいことだ。失われやすいのは、ほぼ常に同じもの——「なぜそう決めたか」である。生成型は作った理由を持たない。実行型は操作の理由を外に出さない。観測型は結果を示すが、決定を記録しない。三つの型を併用したスタックは、施策の物量と速度を手に入れる一方で、判断の履歴だけがどの型にも引き取られずに落ちる、という構造的な欠落を抱えやすい。

具体的な週次の風景を想像してほしい。月曜、観測型のダッシュボードで先週の数字を確認する。数字を見て、担当者が頭の中で仮説を立て、生成型に新しい素材を作らせ、実行型の設定をいくつか変える。金曜、数字は少し動いている。この一週間で、データは観測型に、素材は生成型の履歴に、操作は実行型のログに残った。だが「月曜の数字から何を読み取り、なぜその変更を選んだのか」——一週間で最も価値のある情報——だけは、どのツールにも残っていない。半年後に「あの配分はなぜああなったのか」を辿ろうとしたとき、残っているのは数字と成果物だけで、判断は残っていない。担当者が交代すれば、組織はその期間の学習をまるごと失う。

これは特定のツールの怠慢ではなく、分業の設計の問題である。だからこそ、責めるべき相手を探すのではなく、設計で埋めるべき欠落として扱うのが正しい。

### 2.5.4 私たちの位置

この地図の上で、Start-X の OSS 群と商用の Marketing-OS が立つのは、観測型と判断構造化型の領域である。私たちは実行型を作らない。自動投稿・自動入札・自動最適化は、OSS でも商用でも提供しない。

これは実行型を否定する立場ではない。実行は他の型のツールが担い、私たちはその手前と後ろ——診断・評価・優先順位付けと、判断の記録・照合——を担う。スタックの中での共存関係であり、実行型・生成型が強力になるほど、判断を残す層の必要性はむしろ増す。

私たちの提供物をこの地図に置くと、次のようになる。OSS の三本柱（SEO 編・広告編・SNS 編）は、診断・評価・構造化——観測型の入口と判断構造化型の骨格——を、フォーク可能な素材として提供する。商用の Marketing-OS は、その骨格を組織の運用に載せるための統合体験を提供する。どちらの側でも、実行だけは引き受けない。

この位置取りは、第 3 章で述べる境界線と同じ原則から導かれている。OSS は素材と型を提供し、商用は運用と責任の束ね方を提供する。どちらも「意思決定の所在を利用者の側に残す」ために設計されており、実行を代行しないことはその設計上の帰結である。地図の上の位置と、OSS と商用の境界線は、別々の決定ではなく同じ一つの決定の二つの面である。

### 2.5.5 確かめる問い

自分たちのツール構成をこの地図に置いてみるために、次の三つの問いを勧める。

1. 先月行った意思決定のうち、根拠を後から辿れるものは何件あるか。
2. いま使っているツールを四つの型に置いたとき、判断構造化型の位置に何かあるか。それとも空席か。
3. ツールを一つ解約したと想像したとき、失われるのは「作業」か、それとも「判断の履歴」か。

答えに正解はない。一問目がゼロ件でも、それは怠慢の証明ではなく、いまのスタックが判断を残す設計になっていないという事実の確認である。三つの問いが照らすのは優劣ではなく、自分たちの構成のどこに空席があるかだ。空席を埋めるかどうか、どう埋めるかは、それぞれの組織の判断である。

問いに答えるための道具は、このリポジトリから始められる。[SEO 編](./seo/README.md)・[広告編](./ads/README.md)・[SNS 編](./social/README.md)の各診断は、観測型と判断構造化型の入口として設計されている。導入手順は [docs/QUICKSTART.md](./docs/QUICKSTART.md) を参照。

## 2.5 The Map of AI Marketing

### 2.5.1 Why a Map

By "map" we mean a structural chart of AI marketing tools—classified not by service names, but by what each tool takes over and what it leaves in your hands.

Tools that lead with AI are multiplying fast: some promise automated posting and delivery, some promise optimized ad operations, some mass-produce creative, some visualize outcomes. An understanding built on names goes stale as names churn. Structure outlives names. So we draw a map of structures, not a catalog of names.

As tools multiply, selection debates drift toward "which one is best." The better question is: what part of decision-making does this tool take over, and what does it leave with us? Adopt tools without that question, and a quiet condition sets in—campaigns keep running while no one can explain why decisions were made. We call this the muddying of judgment. The purpose of the map is not to rank tools. It is to make visible where, in your stack, judgment is retained and where it disappears.

This map evaluates and recommends no specific service. It names only types and structures, and no type is superior to another.

### 2.5.2 The Four Types

There is only one axis of classification: which part of the practice of marketing does the tool take over? Does it take over operations, production, or the grasp of facts—or does it support the recording and cross-checking of judgment? Seen along this axis, AI marketing tools fall broadly into four types. Many services span several, but each has a center of gravity in one. If you are unsure where a tool you use belongs, ask: if this tool stopped, what would stop? If delivery stops, it is execution; if production stops, generation; if your grasp of facts stops, observation; if the record of decisions stops, decision-structuring.

**Execution.** An execution-type tool performs operations themselves—posting, bidding, delivery, uploading—automatically, in place of a person. Structurally, you supply targets and an algorithm takes over the individual operational decisions (when, where, how much). Its strength is running operations at a frequency and scale no human team can sustain. Its inherent risk is that the rationale behind operations becomes hard to see from outside—the black box—so organizations lose the ability to explain why budgets landed where they did: the muddying of judgment. This is less a defect of the type than a trade-off structurally inherent in delegating execution.

**Generation.** A generation-type tool produces creative—copy, images, video—with AI. Structurally, it takes prompts and past material as input and outputs artifacts. Its strength is speed and volume of production. Its inherent risk is homogenization: similar outputs emerge from shared underlying technology, so expression converges across the market. The source of differentiation then shifts from "what can we make" to "what do we choose, and why." The more you rely on generation, the scarcer the judgment of selection becomes.

**Observation.** An observation-type tool captures what happened—measurement, attribution, visualization. Structurally, it collects behavioral data and presents it as organized metrics. Its strength is separating the grasp of facts from human memory and intuition. Its inherent risk is that the numbers remain while the judgment does not: dashboards tell you what happened, but what you decided upon seeing them—and why—vanishes somewhere else, usually inside someone's head.

**Decision-structuring.** A decision-structuring tool records and structures decisions themselves, together with their rationale, so they can be revisited and cross-checked later. Structurally, it accumulates the process of diagnosis, evaluation, and prioritization in reusable form. Its strength is moving judgment from individual tacit knowledge into organizational assets. Its inherent risk is that recording demands discipline—unwritten decisions are never structured—and without operating habits it becomes an empty vessel.

Again: no type is superior. Each takes on different work, takes over different things, and leaves different things in your hands.

### 2.5.3 Combining Types

In practice, no stack is complete with a single type. Create with generation, deliver with execution, measure with observation—the combination itself is natural and nothing to reject. For volume and speed, it carries most of the work.

The problem is what tends to fall through the seams. What is lost is almost always the same thing: why we decided. Generation carries no reason for what it made. Execution keeps its operational reasoning inside. Observation shows results but records no decisions. A stack combining the three gains volume and speed while the history of judgment alone drops through, claimed by none of the types.

Picture an ordinary week. Monday: you review last week's numbers on an observation dashboard. Someone forms a hypothesis in their head, has a generation tool produce new material, and adjusts a few settings in an execution tool. Friday: the numbers have moved a little. Over that week, the data stayed with observation, the material in generation's history, the operations in execution's logs. But what was read from Monday's numbers, and why those particular changes were chosen—the single most valuable piece of information the week produced—remains in no tool at all. Six months later, when you try to trace why a budget split ended up as it did, what remains are numbers and artifacts—not the judgment. And when the person in charge moves on, the organization loses that entire period of learning at once.

This is not the negligence of any particular tool; it is a property of how the division of labor is designed. Which is why the right response is not to look for someone to blame, but to treat it as a gap to be closed by design.

### 2.5.4 Where We Stand

On this map, Start-X's open-source tools and the commercial Marketing-OS stand in the territory of observation and decision-structuring. We do not build execution: no auto-posting, no auto-bidding, no auto-optimization—neither in OSS nor commercially.

This is not a stance against execution. Other tools execute; we take the ground before and after—diagnosis, evaluation, prioritization, and the recording and cross-checking of decisions. It is a coexistence within the stack: the stronger execution and generation become, the more necessary the layer that retains judgment.

Placed on the map, our offerings look like this. The three OSS pillars (SEO, Ads, Social) provide diagnosis, evaluation, and structuring—the entry point of observation and the skeleton of decision-structuring—as forkable material. Commercial Marketing-OS provides the integrated experience for running that skeleton at organizational scale. On neither side do we take on execution.

This position follows from the same principle as the boundary in Chapter 3. Open source offers material and patterns; commercial offers how operations and accountability are bundled. Both are designed to keep ownership of decisions on the user's side, and not executing on your behalf is a consequence of that design. Our place on the map and the OSS–commercial boundary are not separate decisions; they are two faces of the same one.

### 2.5.5 Questions to Ask

To place your own stack on this map, we suggest three questions.

1. Of the decisions you made last month, how many have a rationale you can trace after the fact?
2. If you place your current tools on the four types, is anything standing in the decision-structuring position—or is that seat empty?
3. If you imagine canceling one tool, what would you lose: work, or the history of judgment?

There are no correct answers. If the first question comes back at zero, that is not proof of negligence—it is confirmation that your current stack was not designed to retain judgment. What the three questions illuminate is not merit but vacancy: where in your configuration a seat stands empty. Whether and how to fill it is each organization's own decision.

The tools for answering start in this repository. The diagnostics in [SEO](./seo/README.md), [Ads](./ads/README.md), and [Social](./social/README.md) are designed as entry points to observation and decision-structuring. See [docs/QUICKSTART.md](./docs/QUICKSTART.md) to get started.

---

## 3. Marketing-OS との境界線

OSS で提供するものは、再利用可能な部品、診断、テンプレート、CLI／ライブラリ、そして本 Manifesto に代表される思想の言語化である。利用者はフォークや組み込みを通じて、自社のスタックやプロセスに接続できる。

商用の Marketing-OS は、意思決定の OS としての統合体験を提供する。提供形態は二つあり、両者は並列であって階層関係にはない。一つは **AI CMO** — マーケティングの意思決定を支援するセルフサーブ SaaS である。もう一つは **BPO** — 実行を人が代行するサービスである。BPO は AI CMO の上位プランではなく、別軸の選択肢である。

AI CMO は「実行しない頭脳」として設計される。診断・評価・優先順位付け・ブリーフまでを担い、自動投稿・自動入稿・自動最適化のような自律実行は行わない。実際の実行は利用者自身が担うか、委ねたい場合に限り BPO（人の手）が引き受ける。自動実行をしないことは制約ではなく、意思決定の所在を利用者側に残すための設計上の選択である。

AI CMO のプランは **LIGHT / STANDARD / GROWTH / PRO** の 4 階層で固定されている。ここで肝心なのは、使える機能の「形」を決めるのはプランではなくビジネスタイプ（現在 9 種）だという点である。プランが変えるのはクォータ（利用量の上限）であって、機能の種類ではない。上位プランへ移っても「別の頭脳」に切り替わるのではなく、同じ頭脳をより多く使えるようになる。

つまり、OSS は「素材と型」、商用は「運用と責任の束ね方」という住み分けである。

この線引きは二つの価値を同時に守る。第一に、競合が商用成果物を無条件に取り込んで再販する余地を狭め、持続可能な開発投資を守る。第二に、利用者がベンダーロックなき入り口を持てるようにする。両方を使う場合のメリットは明確である。OSS で早期検証と内製接続を行い、商用で組織規模の運用と責任分界を引き受ける。逆に、OSS のみで十分なチームもある。その選択を尊重する。

境界は固定ではない。ロードマップと合意形成に基づき、OSS 側へ下ろすもの・商用側に残すものは見直す。ただし、見直しのたびに理由を公開する。

## 3. The Marketing-OS Boundary

Open source supplies reusable parts, diagnostics, templates, CLIs and libraries, and conceptual writing like this Manifesto. Adopters can fork or embed and connect to their own stack and process.

Commercial Marketing-OS supplies the integrated experience of a decision OS through two parallel offerings—neither one a tier of the other. The first is **AI CMO**, a self-serve SaaS that supports marketing decisions. The second is **BPO**, where people carry out execution on your behalf. BPO is not a higher plan of AI CMO; it is a separate axis of engagement.

AI CMO is designed as a "brain that does not execute." It handles diagnosis, evaluation, prioritization, and briefs, but performs no autonomous execution—no auto-posting, auto-uploading, or auto-optimization. Actual execution stays with the user, or, only when delegated, is taken on by BPO (human hands). Not executing is not a limitation; it is a deliberate design choice that keeps ownership of decisions on the user's side.

AI CMO plans are fixed at four tiers: **LIGHT / STANDARD / GROWTH / PRO**. The key point is that the *shape* of available features is determined by business type (currently nine), not by plan. Plans change quotas (usage ceilings), not the kinds of features. Moving to a higher plan does not swap in "a different brain"; it lets you use the same brain more.

In short, open source offers material and patterns; commercial offers how operations and accountability are bundled.

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

Phase 1(2026年5月):OSS 基盤と Manifesto の公開 — 完了。
Phase 2(6月):商用 Marketing-OS 本体への集中。OSS は最小メンテナンス — 実質完了。
Phase 3(SEO 編 v0.1 CLI):完了(2026年6月、当初想定より前倒し)。
  [@start-x-work/mos-seo](https://www.npmjs.com/package/@start-x-work/mos-seo) として npm 公開。LLMO/AEO 診断・サイト診断・
  コンテンツブリーフ・キーワード意図マッピングを CLI で提供。
Phase 4(SEO 編 Web UI):完了(2026年6月、当初想定より前倒し)。
  [Cloudflare Pages](https://marketing-os-seo.pages.dev) 上で公開。CLI と同じ診断を Web から利用可能。
Phase 5(SEO v1.0 + 広告編準備):完了(2026年6月、前倒し)。
  `@start-x-work/mos-kit` 抽出(N4)・SEO v1.1.1 への依存切替・広告 API 調査(N3)を完了。
Phase 6(広告編 v0.1):完了(2026年6月、前倒し)。
  CLI `@start-x-work/mos-ads` v0.1.2 — キャンペーン構造診断・判断ログ・クリエイティブ評価・Yahoo 読み取り。
  Web UI は [marketing-os-ads.pages.dev](https://marketing-os-ads.pages.dev) で公開(N7)。
Phase 7(SNS 編 v0.1):完了(2026年6月、前倒し)。
  CLI `@start-x-work/mos-social` v0.1 — 投稿評価・カレンダー診断・アカウント診断。自動投稿なし。
  実装リポ: [marketing-os-social](https://github.com/start-x-work/marketing-os-social)。
  Web UI: [marketing-os-social.pages.dev](https://marketing-os-social.pages.dev) (N8)。
統合 CLI(N9):完了(2026年6月)。
  `@start-x-work/marketing-os` v0.1.1 — SEO / 広告 / SNS を `marketing-os <pillar> <command>` で束ねる。
  実装リポ: [marketing-os](https://github.com/start-x-work/marketing-os)。
利用者向けクイックスタート: [docs/QUICKSTART.md](./docs/QUICKSTART.md)（CLI・Web BYOK・GSC/Yahoo 連携手順）。
運用設計(BYOK): 各 Web UI の AI API キー・GSC OAuth・Yahoo トークンは利用者がブラウザに保存（sessionStorage）。運営側の Cloudflare AI Secrets は不要。

粗い位相と Gate の詳細は [Start-X OSS マスターロードマップ v3.0](./master_roadmap_v3.md) も参照。月固定のスケジュールではなく、Gate（完成度）で進行を判断する。優先順位が入れ替わる場合は、ロードマップ上で理由を明示する。

日程は前提であり変更されうる。前倒し・後ろ倒しいずれの場合も、判断の理由をこのロードマップ上で明示する。今回 Phase 3・4 を前倒しできたのは、SEO 編の実装が想定より順調に進んだためである。Phase 5〜7 と統合 CLI も同じ理由——共通基盤（mos-kit）の抽出と実装の並列化により、各編が SEO 編の土台を再利用できたこと——で前倒しとなった。

現在地（2026年7月）: OSS 三本柱と統合 CLI の v0.1 スコープは完了。次フェーズは、(1) 横断 docs サイト（E3・任意・未着手。現状は QUICKSTART 群で代替）、(2) 三編 Web の共通 UI 抽出（E2・任意）、(3) コミュニティ運用の継続（metrics 週次記録・Issue / Discussion 対応）である。いずれも日程は前提であり変更されうる。

変更履歴: ロードマップと本文の変更は、理由を添えて [CHANGELOG.md](./CHANGELOG.md) に記録する。直近では 2026年7月4日に第 2.5 章「マーケAIの地図」を追加した（理由は CHANGELOG を参照）。

成果と撤退に関する判断基準は、別紙の運用基準に記す。ここでは、定期的に読み直し、必要なら Manifesto 本文を微修正するプロセスだけを約束する。

## 5. Roadmap

Phase 1 (May 2026): OSS foundation and Manifesto publication — complete.
Phase 2 (June): Focus on commercial Marketing-OS; minimal OSS maintenance — effectively complete.
Phase 3 (SEO pillar v0.1 CLI): complete (June 2026, ahead of the original schedule).
  Published on npm as [@start-x-work/mos-seo](https://www.npmjs.com/package/@start-x-work/mos-seo). The CLI covers LLMO/AEO audit, site audit, content briefs, and keyword intent mapping.
Phase 4 (SEO pillar Web UI): complete (June 2026, ahead of the original schedule).
  Live on [Cloudflare Pages](https://marketing-os-seo.pages.dev). The same diagnostics are available from the web.
Phase 5 (SEO v1.0 + Ads preparation): complete (June 2026, ahead of schedule).
  Extracted `@start-x-work/mos-kit` (N4), migrated SEO to v1.1.1, completed Ads API research (N3).
Phase 6 (Ads pillar v0.1): complete (June 2026, ahead of schedule).
  CLI `@start-x-work/mos-ads` v0.1.2 — campaign structure diagnosis, decision logs, creative evaluation, Yahoo read-only.
  Web UI live at [marketing-os-ads.pages.dev](https://marketing-os-ads.pages.dev) (N7).
Phase 7 (Social pillar v0.1): complete (June 2026, ahead of schedule).
  CLI `@start-x-work/mos-social` v0.1 — post evaluation, calendar diagnosis, account audit. No auto posting.
  Repository: [marketing-os-social](https://github.com/start-x-work/marketing-os-social).
  Web UI: [marketing-os-social.pages.dev](https://marketing-os-social.pages.dev) (N8).
Unified CLI (N9): complete (June 2026).
  `@start-x-work/marketing-os` v0.1.1 — `marketing-os <pillar> <command>` across SEO, Ads, and Social.
  Repository: [marketing-os](https://github.com/start-x-work/marketing-os).
User quickstart: [docs/QUICKSTART.md](./docs/QUICKSTART.md) (CLI, Web BYOK, GSC/Yahoo).
Operations (BYOK): AI keys, GSC OAuth, and Yahoo tokens are stored in the user’s browser (sessionStorage). No operator-side Cloudflare AI secrets required.

For gates and finer-grained tracking, see the [Start-X OSS Master Roadmap v3.0](./master_roadmap_v3.md). We track progress by gates rather than fixed monthly dates. If priorities shift, we will state the reason on the roadmap itself.

Dates are assumptions and may change. Whenever we move ahead or defer, we will explain the rationale here. Phase 3 and 4 shipped early because SEO implementation progressed faster than expected. Phases 5–7 and the unified CLI shipped early for the same reason: extracting the shared foundation (mos-kit) and parallelizing implementation let each pillar reuse the SEO groundwork.

Where we are (July 2026): the v0.1 scope for the three OSS pillars and the unified CLI is complete. Next up: (1) a cross-repository docs site (E3, optional, not started; QUICKSTART docs serve in the meantime), (2) shared web UI extraction across the three pillars (E2, optional), and (3) ongoing community operations (weekly metrics, Issues / Discussions). All dates remain assumptions and may change.

Change log: roadmap and body changes are recorded with rationale in [CHANGELOG.md](./CHANGELOG.md). Most recently, Chapter 2.5 "The Map of AI Marketing" was added on July 4, 2026 (see CHANGELOG for the rationale).

Success and retreat criteria live in operational documentation. Here we only commit to periodic review and small Manifesto edits when feedback warrants them.

---

## 6. 参加方法

参加は次の四種類が主である。**Star** は関心の可視化とリリース通知の助けになる。**Discussions** は思想・ロードマップ・一般的な問いの場として推奨する。**Issue** はバグや機能要望など、トラッキングが有効なもの向けである（リポジトリごとにテンプレートあり）。**Pull Request** は改善案の具現化に使ってほしい。大きな変更ほど、事前に Discussion で方向性を共有いただけるとスムーズです。

**地図への貢献。** 第 2.5 章の分類（四つの型）は固定ではない。既存の型に収まらないツールの構造や、型の境界にまたがる事例に気づいた場合は、Issue での報告を歓迎する。ただし、この地図は網羅カタログではない。個別のサービス名・ベンダー名の掲載要望には応じず、いただいた知見は名前を持たない「型」と「構造」に一般化したうえで統合する。

はじめての方は [CONTRIBUTING.md](./CONTRIBUTING.md)（貢献の手順と執筆上の約束）をご覧ください。日本語と英語のどちらでも歓迎します。言語が混在するスレッドも問題ありません。行動規範は [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md)（Contributor Covenant 2.1）に従います。セキュリティに関わる報告は [SECURITY.md](./SECURITY.md) の手順で。

商用 Marketing-OS の契約・価格・個別支援については、[marketing-os.jp](https://marketing-os.jp) 側の導線をご利用ください。

## 6. How to Contribute

Four primary paths: **Stars** signal interest and help with release awareness. **Discussions** are best for ideas, roadmap questions, and general conversation. **Issues** suit bugs and feature requests that benefit from tracking (templates vary by repository). **Pull Requests** carry concrete improvements—larger changes go smoother with an upfront Discussion.

**Contributing to the Map.** The taxonomy in Chapter 2.5 (the four types) is not frozen. If you encounter tool structures that fit none of the types, or cases straddling their boundaries, Issue reports are welcome. Note, however, that the map is not an exhaustive catalog: we do not list individual service or vendor names, and contributed insight is generalized into unnamed types and structures before being integrated.

New here? Start with [CONTRIBUTING.md](./CONTRIBUTING.md) (how to contribute and the writing constraints). Japanese and English are both welcome; mixed-language threads are fine. We follow the [Code of Conduct](./CODE_OF_CONDUCT.md) (Contributor Covenant 2.1). For security-related reports, see [SECURITY.md](./SECURITY.md).

For contracts, pricing, and tailored support for commercial Marketing-OS, please use the paths published on [marketing-os.jp](https://marketing-os.jp).

---

## Phase 1 運用メモ（指示書 v2.0）

- **実施記録・チェックリスト:** [templates/setup-history-v2.md](./templates/setup-history-v2.md)  
- **付録A（事前判断の記録）:** [templates/appendix-a-decisions.md](./templates/appendix-a-decisions.md)  
- **付録D（メトリクス記録）:** [templates/metrics.md](./templates/metrics.md)  
- **付録E（公開アナウンス下書き）:** [templates/announcement-drafts/](./templates/announcement-drafts/)  
- **コミット署名（付録A-8）:** [templates/gpg-commit-signing.md](./templates/gpg-commit-signing.md)

🔗 [marketing-os.jp](https://marketing-os.jp) · Organization: [start-x-work](https://github.com/start-x-work)
