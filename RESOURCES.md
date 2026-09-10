# 🛠️ Open-Source Fairness & Bias-Auditing Projects

Curated for the *AI Ethics & Social Justice Podcast* — live, active repositories where real debates about fairness measurement are happening right now. Compiled during research for the episode on technology and social justice.

These are the projects I found, read, and chose to feature because they are maintained, metric-rich, and genuinely contested in their issue trackers.

---

## 1. [AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360) — Trusted-AI (IBM Research)

| | |
|---|---|
| ⭐ Stars | ~2,860 |
| 🍴 Forks | Active (check repo) |
| 👥 Contributors | IBM Research + academic maintainers |
| 📝 License | Apache-2.0 |
| 🌐 Docs | [aif360.readthedocs.io](https://aif360.readthedocs.io/en/stable/) |

**What it does:** The most mature, comprehensive open-source bias-detection and mitigation toolkit. Ships **70+ fairness metrics** and **15+ debiasing algorithms** covering pre-processing, in-processing, and post-processing. Includes a Jupyter notebook tutorial layer and adapters for tabular data.

**Why it matters:** AIF360 is the reference implementation many journalists and policymakers reach for when they want "the list" of fairness metrics. That authority is exactly what makes its internal debates important — when a metric's definition is disputed inside AIF360, it ripples into every article, audit, and courtroom presentation that cites it. See [DEBATES.md](#debatesmd) for one live example.

**Podcast angle:** The project is mathematically deep but policy-facing. A dream case study for the show: a toolkit that literally *enumerates* fairness, and yet the act of enumeration is itself contested.

---

## 2. [Fairlearn](https://github.com/fairlearn/fairlearn) — Microsoft

| | |
|---|---|
| ⭐ Stars | ~2,280 |
| 🍴 Forks | Active (check repo) |
| 👥 Contributors | Core team: Microsoft + academic maintainers (Tamara Atanasoska, Roman Lutz, others) |
| 📝 License | MIT |
| 📖 Docs | [fairlearn.org](https://fairlearn.org) |

**What it does:** A widely-cited Python library for fairness assessment and mitigation. Implements demographic parity, equalized odds, counterfactual fairness, and constraint-based reduction (ExponentiatedGradient, ThresholdOptimizer). Plugs directly into scikit-learn, PyTorch, and pandas pipelines.

**Why it matters:** Fairlearn is explicit that fairness is a **sociotechnical** challenge, not a plug-and-play checklist. Its maintainers famously do **not** hard-code a list of sensitive features in the library — any column can be supplied as sensitive. That design decision is itself a philosophical statement, and a frequent source of boundary disputes (see [fairlearn#1625](https://github.com/fairlearn/fairlearn/issues/1625) on whether "species" belongs as a protected axis).

**Podcast angle:** It's a portrait of a Microsoft-backed "community" project negotiating *what it should protect*. Demands to expand scope (e.g., NLP/speciesist bias) keep colliding with its architectural identity as a general tabular-fairness engine.

---

## 3. [AI Bias Audit Tool](https://github.com/srhill12/ai-bias-audit-tool) — Steven Hill (Purdue)

| | |
|---|---|
| ⭐ Stars | Small (check repo) |
| 🍴 Forks | — |
| 👥 Contributors | 1 (author) |
| 📝 License | Not specified |
| 🛠️ Stack | Python, Streamlit |

**What it does:** A Streamlit web app that audits the **COMPAS Recidivism Risk Score** — the algorithm ProPublica (2016) found to show significant racial bias. It computes industry-standard fairness metrics (demographic parity difference, disparate impact / 80% rule, false positive/negative rate parity, equalized odds, accuracy by group), visualizes disparities, and emits a plain-text audit report aligned with the **NIST AI Risk Management Framework (MAP · MEA · MGO)**.

**Why it matters:** This is the "applied" counterweight to the big toolkits. It takes one high-stakes, real-world criminal-justice algorithm and runs the full measurement pipeline end-to-end. It's a great demo of how a fairness *audit* is actually performed — and where it still falls short (personal, single-maintainer project with no open issues yet).

**Podcast angle:** COMPAS is the case that _started_ the modern fairness-in-criminal-justice conversation. Running it through a NIST-AI-RMF-aligned audit lens lets the episode bridge the ProPublica journalism of 2016 with today's "governance" vocabulary.

---

## Also discovered (worth a look / future episodes)

These turned up in research and are active enough to track, but didn't make the feature three:

| Project | Owner | Angle |
|---|---|---|
| [Bias-and-Fairness-Auditing-Tool](https://github.com/rara1803/Bias-and-Fairness-Auditing-Tool) | rara1803 | End-to-end bias detection + mitigation, multi-model |
| [Algorithmic-Fairness-Toolkit](https://github.com/NikhilRajBharti26-gif/Algorithmic-Fairness-Toolkit) | NikhilRajBharti26-gif | Bias auditing + mitigation across demographic groups |
| [Informations-Ethics (Ethical-Data-Checker)](https://github.com/matthaioum/Informations-Ethics---Ethical-Data-Checker) | matthaioum | R Shiny pre-ML dataset audit: bias detection, demographic parity, disparate impact |
| [compas-fairness-audit](https://github.com/leonardphokane/compas-fairness-audit) | leonardphokane | Notebook-based COMPAS audit with policy guidelines |
| [Insurance-premium-fairness-analyser](https://github.com/harshitha53143/Insurance-premium-fairness-analyser-) | harshitha53143 | Automated audit of insurance pricing for demographic bias & proxy discrimination |
| [AI-Ethics-Fairness-Audit](https://github.com/E-Macharia/AI-Ethics-Fairness-Audit) | E-Macharia | Case studies on hiring tools + facial recognition |

---

## How to Add a Project

1. Fork this repo
2. Add your entry following the table format above
3. Include: name, owner, GitHub link, key stats (stars/forks/license), what it does, why it matters, and a podcast angle
4. Open a PR describing what you added and why

We prioritize tools that:
- Have been updated within the last 12 months
- Carry a clear open-source license
- Center communities most affected by algorithmic harm, not only technocrats
- Surface genuine disagreements about how to measure fairness or whom to serve
