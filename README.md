# 🎙️ AI Ethics & Social Justice — Podcast Resource Hub

A **crowdsourced hub** for the *AI Ethics & Social Justice* podcast — curating open-source tools, live debates, and community resources on algorithmic fairness, bias auditing, and the ethics of automated decision-making.

## 🎯 What This Is

This repo is a **living research companion** for our podcast episodes. We track:

- **Active open-source projects** building fairness toolkits, bias auditors, and governance platforms
- **Real debates happening in those projects** — disagreements over how to measure fairness, who tools should serve, and what "fair" even means
- **Community discussion** — pull requests, issues, and explainers that surface the tensions between technical rigor and social justice

We believe the most interesting stories about AI ethics aren't in the headlines — they're in the **open issue threads** of the repos that shape real policy. This hub makes those debates accessible.

## 📂 Repository Structure

```
ai-ethics-podcast-resources/
├── RESOURCES.md    # Curated links to open-source fairness projects
├── DEBATES.md      # Summaries of ongoing controversies from issue threads
├── PODCAST.md      # Episode planner with hooks, key questions, and talking points
└── README.md       # This file
```

## 🔍 What You'll Find

### Active Fairness Toolkits
We track **4+ active open-source projects** per episode, spanning:
- **Bias auditing pipelines** — end-to-end fairness audits on real datasets (COMPAS, hiring, lending, healthcare)
- **Fairness metric libraries** — implementations of demographic parity, equalized odds, predictive parity, counterfactual fairness
- **Governance & compliance platforms** — evidence-grade AI assurance for regulatory frameworks (EU AI Act, NIST AI RMF)
- **Academic research tools** — smaller, principled libraries that prioritize clarity over comprehensiveness
- **Community-driven API frameworks** — projects like FairLearn that ask "who should this tool serve?" at the API level

### Live Debates
Each episode zooms in on a **specific ongoing disagreement** from an open issue thread:
- How should we measure the influence of race in a model? (SHAP-based vs. causal approaches)
- Which fairness metric should dominate when they conflict? (Demographic parity vs. equalized odds vs. predictive parity)
- Who bears the cost of a "fair" model — and who decides? (AIF360 #528)
- Can counterfactual fairness ever be computed without a contested causal graph? (Fair-Code #672)
- What does "zero" actually mean in a fairness metric? (AIF360 #528)
- Should fairness tools support intersectional analysis, or is a single scalar enough? (AIF360 #558)
- Should a fairness tool be a general-purpose framework or a specialized instrument? (FairLearn #756 — 74 comments, 5+ years, unresolved)
- What should a fairness tool do when sensitive data is missing? (FairLearn #1725)
- Who does a fairness tool serve — practitioners, researchers, or the communities being audited?

## 🤝 How to Contribute

We welcome contributions from everyone — podcasters, engineers, ethicists, and the just-curious:

1. **Found a great fairness toolkit?** Add it to `RESOURCES.md`
2. **Spotted a real debate in an issue thread?** Summarize it in `DEBATES.md`
3. **Want to discuss an episode topic?** Open an issue with the `discussion` label
4. **Have a counterargument?** Open a PR — fairness debates thrive on disagreement

## 📌 Featured Debates (See `DEBATES.md`)

Each episode centers on a **real, unresolved controversy** from an open-source community. Current featured debates include:

- **The Average-Odds Documentation Bug** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition? After 17 months, a volunteer offered a fix — but no maintainer has merged it.
- **The Intersectional Analysis Gap** (AIF360 #558) — Should fairness tools return a single scalar, or should they break down discrimination by specific group combinations? A volunteer offered to implement the enhancement.
- **The MetricFrame API Design Debate** (FairLearn #756) — 74 comments, 5+ years, unresolved. Should a fairness tool be a general-purpose framework or a specialized instrument?
- **The Missing-Value Contract** (FairLearn #1725) — Should fairness tools be strict or silent when sensitive data is missing?
- **The Metrics Governance Gap** (Aequitas #201) — Even the toolkit built for policymakers can't explain its own metrics.
- **The Versioning Trust Gap** (Aequitas #209) — When a tool ships breaking changes without a version bump, how can anyone trust its output?
- **The Corporate Fairness Question** (Infosys RAI) — When a major IT company builds the fairness tooling, whose definition of "fair" becomes the default?

## 📬 Feedback & Discussion

- **Episode suggestions** — Open an issue with label `episode-suggestion`
- **Debate nominations** — Found a great fairness thread? Open an issue with label `debate-nomination`
- **General discussion** — Open any issue, tag it `discussion`

## 🎙️ Subscribe & Support

- **Episodes** — [PODCAST.md](PODCAST.md) for episode outlines and talking points
- **Resources** — [RESOURCES.md](RESOURCES.md) for the full project catalog
- **Debates** — [DEBATES.md](DEBATES.md) for deep dives into the controversies

## 📜 License

This resource hub is released under the [MIT License](LICENSE). Individual project links remain the property of their respective authors.

## Repository Structure

### Files

| File | Description |
|---|---|
| `RESOURCES.md` | Curated links to open-source fairness projects, with stars, licenses, and podcast angles |
| `DEBATES.md` | Deep summaries of ongoing controversies from GitHub issue threads |
| `PODCAST.md` | Episode planner with hooks, key questions, guest suggestions, and talking points |
| `README.md` | This file — hub overview and contribution guide |

## 🔗 Quick Links

- [📚 Resources](RESOURCES.md) — Project catalog
- [⚖️ Debates](DEBATES.md) — Controversy summaries
- [🎙️ Episodes](PODCAST.md) — Episode planner
- [🔍 AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The average_odds_difference documentation bug
- [🔍 FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — The MetricFrame API design debate

*Contributors: Add your favourite fairness project, debate, or episode idea — this hub is crowd-sourced by design.*