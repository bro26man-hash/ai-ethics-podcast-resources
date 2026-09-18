# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of **active open-source projects** related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects (Episode Research)

### 1. [AI Fairness 360 (AIF360) — IBM / Trusted-AI](https://github.com/Trusted-AI/AIF360) — *Comprehensive Fairness Metric Library & Mitigation*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Language** | Python (also R) |
| **License** | Apache-2.0 |
| **Last Updated** | September 2026 |
| **Maintainer** | IBM Research (Trusted-AI org) |

**What it does:** An extensible open-source library containing techniques developed by the research community to detect and mitigate bias in machine learning models throughout the AI application lifecycle. Includes 15+ bias mitigation algorithms (Optimized Preprocessing, Disparate Impact Remover, Equalized Odds Postprocessing, Reweighing, Adversarial Debiasing, etc.) and a comprehensive set of fairness metrics (Demographic Parity, Equalized Odds, Predictive Parity, Generalized Entropy Index, Differential Fairness, and more).

**Why it matters for the podcast:** AIF360 is the most widely cited fairness toolkit in production and government contexts. Its metrics are referenced in regulatory filings, court briefs, and compliance reports. But as Issue #528 reveals, the documentation for its `average_odds_difference` metric has stated for **2+ years** that "a value of 0 indicates equality of odds" — which is mathematically incorrect. This isn't just a typo; it's a definitional error in the most-cited fairness toolkit.

**🔴 Live Debate:** See [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The `average_odds_difference` docstring says "A value of 0 indicates equality of odds," but the issue author proved with a diagram that any pair of points on a specific line have `average_odds_difference=0` yet do **not** fulfill equalized odds. The error also appears on IBM's own website. After 17 months of no maintainer response, a volunteer (Hanabi9248) offered a concrete correction in September 2026 — but the issue remains **unassigned and unmerged**. See `DEBATES.md` for the full summary.

```bash
pip install aif360
from aif360.metrics import ClassificationMetric
# Try the metric whose docstring is wrong — then read the issue
```

**Podcast angle:** When the most-cited fairness toolkit ships a docstring that conflates a metric with the concept it's supposed to measure, who owns the definition of "fair"? And is a volunteer-submitted fix without a maintainer merge actually authoritative?

---

### 2. [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn) — *Fairness Assessment & Mitigation*

| | |
|---|---|
| **Stars** | 2,286 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Microsoft (fairlearn org) |

**What it does:** A Python package to assess and improve the fairness of ML models. Contains fairness metrics for group fairness assessment (Demographic Parity, Equalized Odds, Predictive Parity, Conditional Demographic Parity) and mitigation algorithms (Exponentiated Gradient, Grid Search, ThresholdOptimizer). Tight scikit-learn integration makes it the go-to for practitioners in the Microsoft/MLOps ecosystem. Explicitly defines fairness in terms of *harms* — allocation harms (opportunity, resources, information) and quality-of-service harms.

**Why it matters for the podcast:** Fairlean is the philosophical counterpoint to AIF360. Where AIF360 is comprehensive and research-oriented, Fairlean is opinionated about fairness as harm reduction. Its README explicitly states: *"Fairness is fundamentally a sociotechnical challenge. Many aspects of fairness, such as justice and due process, are not captured by quantitative fairness metrics."* This self-awareness about the limits of its own tools is rare.

**🔴 Live Debate:** See [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — MetricFrame API design: should the tool support metrics beyond classification/regression, even if it makes the API more complex? The 74-comment thread (open since April 2021) reveals a deep divide between simplicity and generality. See `DEBATES.md` for the full summary.

```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame
from fairlearn.reductions import ExponentiatedGradient
```

**Podcast angle:** Fairlearn says it can't capture justice — so what *does* it optimize for? And when a tool's own documentation says "we know our limits," should that change how we regulate it?

---

### 3. [Infosys Responsible AI Toolkit — Infosys](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — *Enterprise-Grade AI Governance & Bias Detection*

| | |
|---|---|
| **Stars** | 310 ⭐ |
| **Language** | Python |
| **License** | Apache-2.0 |
| **Last Updated** | September 2026 |
| **Maintainer** | Infosys (commercial consultancy) |

**What it does:** A comprehensive responsible AI toolkit covering **safety, security, explainability, fairness, bias, and hallucination detection** — not just fairness, but the full spectrum of AI risk. Designed for enterprise deployment, it bridges the gap between academic fairness research and production MLOps pipelines. Includes tools for bias detection in NLP, computer vision, and tabular data.

**Why it matters for the podcast:** This is what happens when a **Big 5 consultancy** builds a fairness toolkit. Unlike AIF360 (IBM Research) and FairLearn (Microsoft), the Infosys toolkit is built by a company that *deploys* AI systems for clients. The lens is governance-first: not just "is this model fair?" but "can you prove it to a regulator?" The inclusion of hallucination detection alongside fairness signals that enterprise AI risk is Holistic — fairness is one pillar, not the whole structure.

**Podcast angle:** When a consultancy builds a fairness tool, whose interests does it serve — the communities being audited, or the enterprises buying it? Is there a fundamental conflict between "fairness as justice" and "fairness as compliance"?

---

## 📖 Bonus: Related Projects

### [Fair-Code — yakew7](https://github.com/yakew7/Fair-Code) — *7 Open-Source Algorithmic Bias Audits*

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Language** | Python / HTML |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Yash Kewlani (solo, with community contributors) |

Seven full fairness audits across criminal justice (COMPAS), hiring, lending, healthcare, welfare eligibility, and tenant screening. Each audit ships as runnable scripts (`unfair.py` / `fair.py`) plus Jupyter notebooks. Includes an Open Dataset Profiler, a benchmark harness with 5 mitigation strategies, 6 fairness metrics with bootstrap CIs, and 61 plain-language explainers.

**🔴 Live Debate:** See [Fair-Code Issue #672](https://github.com/yakew7/Fair-Code/issues/672) — The SHAP Measurement Problem: how should you report race's influence? The answer depends on a denominator choice (top-5 features vs. all features), which is a rhetorical decision disguised as a number.

### [AIBF_API — jbarach2012](https://github.com/jbarach2012/AIBF_API) — *Explainable Bias-Detection Firewall for Hiring ATS*

| | |
|---|---|
| **Stars** | 195 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |

An open-source, explainable bias-detection firewall for Applicant Tracking Systems. AIBF intercepts an ATS scoring decision, measures how much of it was driven by protected-attribute proxies rather than merit, explains why in plain language, and flags biased decisions for human review.

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Stars | Focus | Best For |
|---|---|---|---|
| **AIF360** | 2,866 ⭐ | Comprehensive metric library + mitigation | Understanding how fairness is defined in production/government contexts; **live debate in Issue #528** |
| **FairLearn** | 2,286 ⭐ | Fairness assessment + mitigation | Practitioners in Microsoft/MLOps ecosystems; **live debate in Issue #756** |
| **Infosys RAI** | 310 ⭐ | Enterprise AI governance (fairness + safety + hallucination) | The enterprise compliance lens; how Big 5 consultancies approach responsible AI |
| **Fair-Code** | 47 ⭐ | 7 full bias audits + dataset profiler + explainers | Domain-specific audits (COMPAS, healthcare, tenant screening); SHAP measurement debate |
| **AIBF_API** | 195 ⭐ | Real-time bias detection in hiring ATS | The hiring-pipeline use case; post-hoc flagging vs. pre-decision interception |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation (see Issue #528 for known errors)
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — Enterprise-grade responsible AI
- [Fair-Code explainers](https://www.thefaircode.xyz) — 61 plain-language fairness explainers with runnable code
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The average_odds_difference documentation bug (open since April 2024)
- [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Extend Empirical Differential Fairness for intersectional analysis (open since Jan 2026)
- [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — MetricFrame API design debate (74 comments, open since 2021)
- [Fair-Code Issue #672](https://github.com/yakew7/Fair-Code/issues/672) — The SHAP Measurement Problem
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainasan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

## 🎙️ How These Projects Connect to the Podcast

Each episode zooms in on a **specific ongoing disagreement** from an open issue thread:

1. **The Average-Odds Documentation Bug** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition? After 17+ months, a volunteer offered a fix — but no maintainer has merged it.
2. **The Intersectional Analysis Gap** (AIF360 #558) — Should fairness tools return a single scalar, or should they break down discrimination by specific group combinations? A volunteer offered to implement the enhancement.
3. **The MetricFrame API Design Debate** (FairLearn #756) — Should a fairness tool support metrics beyond classification, even if it makes the API more complex? Who does the tool serve — practitioners or researchers? 74 comments, 5 months, unresolved.
4. **The SHAP Measurement Problem** (Fair-Code #672) — Does the choice of denominator change the story of racial bias?
5. **The Enterprise vs. Activist Lens** (Infosys RAI) — When a Big 5 consultancy builds a fairness tool, is fairness about justice or compliance?
6. **The Impossibility Triangle** (Fair-Code #665) — When base rates differ, you can't satisfy equalized odds and predictive parity simultaneously. Which metric wins?

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*