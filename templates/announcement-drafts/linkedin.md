# LinkedIn post (English, ~800–1,200 words)

**Audience:** Engineers abroad interested in marketing tooling, LLMO/AEO, and OSS governance.  
**CTA:** Read the Manifesto and join Discussions.

---

Marketing decisions don’t fail because teams lack tools. They fail because priorities, assumptions, and accountability drift across meetings, dashboards, and vendors. As generative interfaces change how buyers discover options, the gap between “what we shipped” and “why we chose this path” widens unless someone keeps editing the structure itself.

I work on **Marketing-OS**, a commercial product that treats marketing as an operating system for decisions—not only campaigns. Today we are also opening part of that thinking and tooling as **open source** under the GitHub organization **start-x-work**.

### Why open source, why now

Open source is not a stunt and not a giveaway of the entire product. It is a deliberate choice to publish **material and patterns**—diagnostics, CLI foundations, and a bilingual **Manifesto**—so teams can verify, fork, and connect what we publish to their own stacks. The commercial layer continues to cover integrated workflows, contractual responsibility, and the “how do we run this every week” experience.

We are explicit about boundaries. Chapter 3 of the Manifesto describes what stays OSS versus what remains commercial. We are explicit about sequencing: **SEO first**, then Ads, then Social. Organic visibility and site structure precondition many downstream choices; shipping SEO v0.1 as a CLI in mid‑2026 is the engineering focus before we expand.

### What is in the repos today

- **manifesto** — the conceptual hub, six chapters in Japanese and English, plus category READMEs for SEO, Ads, and Social.  
- **marketing-os-seo** — the implementation focus; v0.1 CLI targets LLMO/AEO audits, technical SEO checks, content brief support, and core keyword intent mapping.  
- **marketing-os-ads** and **marketing-os-social** — **Coming Soon** placeholders with issue templates that welcome ideas while stating realistic start windows (2027 Q1 / Q2).

Apache 2.0 is the default license; we document why in `licenses.md`. Security contact and Contributor Covenant enforcement paths are wired through the organization’s `.github` repository.

### Why Japan still matters for this stack

Much marketing software assumes US-centric defaults. Japan’s B2B buying behavior, agency relationships, and regulatory nuances still shape how teams plan search, paid media, and owned channels. We are not claiming a single country “owns” the problem—but we do believe a credible OSS layer for **LLMO/AEO** and **technical SEO**, written with bilingual documentation, helps teams in Asia-Pacific connect local practice to global tooling. If you maintain multilingual sites or answer to both HQ and regional marketing, we would like your review of the Manifesto’s assumptions.

### Reliability, security, and governance

Each repository has **private vulnerability reporting** enabled at the GitHub level, alongside the email path documented in `SECURITY.md`. Branch protection on `main` prevents force-push and deletion; we keep reviews lightweight early on so small documentation fixes can land quickly while we stabilize templates.

### How you can participate

If this resonates, start with a **star** for release awareness, read the **Manifesto**, and drop a note in **Discussions**—especially on the first thread we opened for orientation. Issues and PRs are welcome where you have concrete improvements. We keep debate **editorial**: rearrange material, state assumptions, propose alternatives.

If you are evaluating similar tools, compare our explicit **non-goals** for v0.1 (for example, deferring heavy rank-tracking databases) against your internal requirements. Feedback on those trade-offs is particularly valuable before we freeze CLI interfaces.

Links:

- Manifesto: https://github.com/start-x-work/manifesto  
- SEO toolkit repo: https://github.com/start-x-work/marketing-os-seo  
- Commercial product context: https://marketing-os.jp  

We will move the roadmap when priorities shift and say why. If you care about marketing infrastructure that respects both openness and sustainability, I would value your eyes on the text and the code.

### A note on pace

June is intentionally quiet on OSS feature work so we can protect core commercial milestones. That pacing is part of the contract we are trying to model: **open material, sustainable investment**. If you need faster movement on a specific diagnostic, open a Discussion with your constraints—we may not commit immediately, but we will respond editorially, with assumptions spelled out.

If you are building internal glue code around LLMs and search data, consider publishing a minimal reproducer when you open an issue—Japanese or English is fine. That habit shortens the loop between “interesting complaint” and “merged improvement,” especially while the CLI surface is still stabilizing.

Finally, if you are comparing this program to a fully hosted SaaS-only roadmap, expect different trade-offs. We bias toward **inspectable scripts** and **documented boundaries** over magic dashboards. That is less flashy on day one, but it ages better when your stack changes underneath you.

We also welcome **cross-border collaboration** on documentation: if a paragraph in the Manifesto reads awkward in English, propose a PR that keeps the intent but improves the rhythm. Small copy edits are a legitimate form of contribution.

---

（Word count はおおむね 800〜1,200 語レンジを目指して拡張済み。公開前に固有名詞とリンクの最終確認をすること。）
