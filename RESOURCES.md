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

### 2. [Fair-Code — yakew7](https://github.com/yakew7/Fair-Code) — *7 Open-Source Algorithmic Bias Audits*

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Language** | Python / HTML |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Yash Kewlani (solo, with community contributors) |

**What it does:** Seven full fairness audits across criminal justice (COMPAS), hiring, lending, healthcare, welfare eligibility, and tenant screening. Each audit ships as runnable scripts (`unfair.py` / `fair.py`) plus Jupyter notebooks. Includes an Open Dataset Profiler (client-side web tool + CLI), a benchmark harness with 5 mitigation strategies (S0–S4), 6 fairness metrics with bootstrap CIs and permutation tests, and 61 plain-language explainers. Also ships an MCP server for agent tool-calling.

**Why it matters for the podcast:** Fair-Code is the most transparently documented fairness project we found. Every audit follows the same pipeline: train a biased model → measure the fairness gap → identify proxy variables → remove them → retrain → measure again. The results are publishable-grade. The healthcare focus is distinctive — three of seven audits examine bias in medical AI, where the consequences are clinical rather than financial. The tenant screening audit delivers a devastating finding: removing race and all 12 proxy variables only cuts the fairness gap from 6.68% to 5.16%, and it stays statistically significant — because the label itself (re-arrest) is a policed quantity.

**Active issues:**
- [#672](https://github.com/yakew7/Fair-Code/issues/672) — SHAP Measurement Problem: how should you report race's influence? The answer depends on a denominator choice (top-5 features vs. all features), which is a rhetorical decision disguised as a number
- [#665](https://github.com/yakew7/Fair-Code/issues/665) — The Impossibility Triangle: when base rates differ, you can't satisfy equalized odds and predictive parity simultaneously

```bash
git clone https://github.com/yakew7/Fair-Code.git
cd Fair-Code
pip install -r requirements.txt
python COMPAS/unfair.py   # see the bias
python COMPAS/fair.py     # see the fix
```

**Podcast angle:** What happens when the math itself says "you can't have it all"? The impossibility theorem isn't a bug — it's a feature of the underlying mathematics. And the choice of which metric to prioritize is a political decision.

---

### 3. [EqualityML — EqualityAI](https://github.com/EqualityAI/EqualityML) — *Evidence-Based Fairness Tools & Community Collaboration*

| | |
|---|---|
| **Stars** | 35 ⭐ |
| **Language** | Jupyter Notebook / Python (also R) |
| **License** | Apache-2.0 |
| **Last Updated** | April 2026 |
| **Maintainer** | EqualityAI (public-benefit corporation) |

**What it does:** A Python and R package providing fairness metrics (Statistical Parity, Conditional Statistical Parity, Negative Predictive Parity, Equal Opportunity, Balance for Positive/Negative Class, Predictive Parity, Well Calibration, Calibration, Conditional Use Accuracy, Predictive Equality, Equalized Odds, Overall Balance) and bias mitigation methods (Resampling, Reweighting, Disparate Impact Remover, Correlation Remover). Includes a Fairness Metric Selection Questionnaire & Decision Tree to help practitioners choose the right metric. In-processing and post-processing methods are listed as "still under development."

**Why it matters for the podcast:** EqualityML is the only major fairness toolkit built by a **public-benefit corporation** rather than a tech giant or university. Its manifesto-driven approach ("Join our EAI Manifesto!") and Slack community model positions it as a practitioner-first tool. But the "under development" status of in-processing and post-processing methods raises a question: **can a fairness toolkit be complete if it only handles pre-processing?** The answer may depend on whether you believe bias is primarily a data problem (pre-processing) or also a model-level and prediction-level problem.

**Active issues:**
- In-processing and post-processing methods explicitly marked as "still under development"
- No published GitHub issue tracker activity visible — the project's community engagement happens primarily through Slack

```bash
pip install equalityml
from equalityml import FAIR
# See the quick-start example in the README
```

**Podcast angle:** Does the "under development" status of in-processing and post-processing methods mean EqualityML is incomplete — or does it correctly argue that pre-processing is where the real fairness battle is won or lost? And what does it mean that a public-benefit corporation is the steward of this tool?

---

## 📖 Bonus: Related Projects

### [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn) — *Fairness Assessment & Mitigation*

| | |
|---|---|
| **Stars** | 2,286 ⭐ |
| **Language** | Python |
| **License** | MIT |

A Python package to assess and improve the fairness of ML models. Supports key fairness metrics and mitigation algorithms with scikit-learn integration. **🔴 Live Debate:** [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — MetricFrame API design: should the tool support metrics that don't require `y_true` and `y_pred`? The 74-comment thread (open since April 2021) reveals a deep divide between simplicity and generality. See `DEBATES.md` for the full summary.

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
| **Fair-Code** | 47 ⭐ | 7 full bias audits + dataset profiler + explainers | Domain-specific audits (COMPAS, healthcare, tenant screening); SHAP measurement debate |
| **EqualityML** | 35 ⭐ | Pre-processing fairness metrics + mitigation | Public-benefit approach; the "pre-processing only" question |
| **AIBF_API** | 195 ⭐ | Real-time bias detection in hiring ATS | The hiring-pipeline use case; post-hoc flagging vs. pre-decision interception |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation (see Issue #528 for known errors)
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [Fair-Code explainers](https://www.thefaircode.xyz) — 61 plain-language fairness explainers with runnable code
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The average_odds_difference documentation bug (open since April 2024)
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

1. **The Average-Odds Documentation Bug** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition?
2. **The Intersectional Analysis Gap** (AIF360 #558) — Should fairness tools return a single scalar, or should they break down discrimination by specific group combinations?
3. **The MetricFrame API Design Debate** (FairLearn #756) — Should a fairness tool support metrics beyond classification/regression, even if it makes the API more complex? Who does the tool serve — practitioners or researchers?
4. **The SHAP Measurement Problem** (Fair-Code #672) — Does the choice of denominator change the story of racial bias?
5. **The Impossibility Triangle** (Fair-Code #665) — When base rates differ, you can't satisfy equalized odds and predictive parity simultaneously. Which metric wins?
6. **The "Pre-Processing Only" Question** (EqualityML) — Can a fairness toolkit be complete if it only handles pre-processing?

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*