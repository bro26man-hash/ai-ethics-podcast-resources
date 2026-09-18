# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of **active open-source projects** related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects (Episode Research)

### 1. [AIF360 — IBM Research (Trusted-AI)](https://github.com/Trusted-AI/AIF360) — *Comprehensive Fairness Metric Toolkit*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Language** | Python (also R) |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |
| **Maintainer** | IBM Research (Trusted-AI org) |

**What it does:** A comprehensive set of fairness metrics for datasets and machine learning models, with explanations for each metric and algorithms to mitigate bias. Covers over 20 fairness metrics across multiple categories: group fairness (demographic parity, equalized odds, predictive parity), individual fairness, and causal fairness. Includes bias mitigation algorithms for pre-processing, in-processing, and post-processing.

**Why it matters for the podcast:** AIF360 is the most widely deployed fairness toolkit in production ML pipelines. Its metric definitions are the de facto standard for fairness auditing in enterprise and government contexts. When the tool itself mislabels a metric — as in **Issue #528** (see `DEBATES.md`) — the stakes are not academic: courts and regulators reference these definitions.

**Key feature:** The `ClassificationMetric` class provides a unified API for computing fairness metrics, but as Issue #528 reveals, the documentation for `average_odds_difference` misrepresents what "zero" means.

**Active issues:**
- [#528](https://github.com/Trusted-AI/AIF360/issues/528) — `average_odds_difference` docstring wrongly claims "equality of odds" (open since April 2024, 2 👍)
- [#558](https://github.com/Trusted-AI/AIF360/issues/558) — Extend Empirical Differential Fairness metric (open since Jan 2026)
- [#548](https://github.com/Trusted-AI/AIF360/issues/548) — AI Fairness 360 website is down (open since Feb 2025, 7 comments)
- [#526](https://github.com/Trusted-AI/AIF360/issues/526) — Memory Management Issue in ClassificationMetric (open since April 2024)

```bash
pip install aif360
from aif360.metrics import ClassificationMetric
# See metric definitions and use examples in the docs
```

**Podcast angle:** When the most-cited fairness toolkit ships with a documentation error about what "zero" means, who catches it — and who decides what the number means?

---

### 2. [AIBF_API — jbarach2012](https://github.com/jbarach2012/AIBF_API) — *Explainable Bias-Detection Firewall for Hiring ATS*

| | |
|---|---|
| **Stars** | 195 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |
| **Maintainer** | jbarach2012 (independent contributor) |

**What it does:** An open-source, explainable bias-detection firewall for Applicant Tracking Systems. AIBF intercepts an ATS scoring decision, measures how much of it was driven by protected-attribute proxies rather than merit, explains why in plain language, flags biased decisions for human review, and learns from HR feedback. Built on synthetic data — no real candidate data ships with the project.

**Why it matters for the podcast:** This is fairness auditing applied to the hiring pipeline — the domain where 87% of companies screen resumes with AI before a human ever sees them. Unlike the academic toolkits (AIF360, FairLearn), AIBF is built for a specific, high-stakes application: real-time decision interception. It raises a distinct question: **can a "firewall" approach — flagging bias after the decision but before the action — actually protect candidates, or is post-hoc flagging too late?**

**Key feature:** The bias score is the share of a decision's score-movement coming from protected-proxy features, computed via SHAP values against a neutral (attribute-absent) baseline. Decisions above a threshold are flagged with plain-language explanations.

```bash
git clone https://github.com/jbarach2012/AIBF_API
cd aibf
pip install -r requirements.txt
python -m app.services.pipeline_demo  # see a biased resume flagged, a clean one not
```

**Podcast angle:** If a hiring AI flags 34% of a rejected candidate's score as coming from protected-attribute proxies, what happens next? Does the candidate get a real remedy, or just a flag that gets ignored? And who builds the watchdog — the same company that built the hunting dog?

---

### 3. [Fair-Code — yakew7](https://github.com/yakew7/Fair-Code) — *7 Open-Source Algorithmic Bias Audits*

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Language** | Python / HTML |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | yakew7 (independent researcher) |

**What it does:** A research and engineering project that exposes bias in real-world AI systems and demonstrates concrete mitigation strategies. Every audit follows the same pipeline: train a biased model → measure the fairness gap → engineer a fair model → measure again. Seven full audits across criminal justice (COMPAS), hiring, lending, healthcare (insurance denial + readmission), welfare eligibility, and tenant screening. Each ships as runnable `unfair.py` / `fair.py` scripts plus Jupyter notebooks.

**Why it matters for the podcast:** Fair-Code proves that bias is measurable and fixable — but also that removing protected attributes alone isn't enough; proxy variables smuggle the same signal back in. The tenant-screening audit is particularly striking: dropping race and all 12 criminal-history proxies only cuts the fairness gap from 6.68% to 5.16%, and it stays statistically significant — because re-arrest itself is a policed quantity. The real remedy isn't a cleaner feature set; it's questioning whether the score belongs in the decision at all.

**Key features:**
- **Open Dataset Profiler** — audits datasets for demographic representation before any model is trained (web + CLI)
- **Benchmark Harness** — applies one uniform pipeline across all 7 audits with 3 mitigation strategies (S0–S4)
- **61 explainers** — plain-language write-ups of individual fairness concepts, each with runnable detection code
- **MCP server** — 6 tools for coding agents to profile datasets mid-conversation

**Active issues:**
- [#672](https://github.com/yakew7/Fair-Code/issues/672) — SHAP measurement: does the denominator (top-5 features vs. all features) change the story? (see `DEBATES.md`)
- [#654](https://github.com/yakew7/Fair-Code/issues/654) — Counterfactual fairness reproduction: the headline numbers don't match the code (see `DEBATES.md`)
- [#665](https://github.com/yakew7/Fair-Code/issues/665) — COMPAS numbers in the impossibility-theorem explainer are contested

```bash
git clone https://github.com/yakew7/Fair-Code
cd Fair-Code
python COMPAS/unfair.py   # see the bias
python COMPAS/fair.py     # see the fix
```

**Podcast angle:** When the tenant-screening audit shows that even after removing race and all proxies, the gap stays significant — what's the fix? Better feature engineering, or rethinking whether re-arrest scores should determine who gets a lease?

---

### 4. [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn) — *Fairness Assessment & Mitigation*

| | |
|---|---|
| **Stars** | 2,286 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Microsoft (fairlearn org) |

**What it does:** A Python package to assess and improve the fairness of machine learning models. Supports key fairness metrics (demographic parity, equalized odds, predictive parity, etc.) and mitigation algorithms (thresholding, re-weighting, adversarial debiasing). Designed for integration with scikit-learn workflows.

**Why it matters for the podcast:** FairLearn is Microsoft's answer to AIF360 — same core metrics, different design philosophy. Where AIF360 is an IBM Research project with academic roots, FairLearn is built for practical ML pipelines inside Microsoft's ecosystem. The two toolkits sometimes define and compute the same metrics slightly differently, raising the question: **if two mainstream fairness tools disagree on a metric's definition, which one is "right"?**

**🔴 Live Debate:** See [FairLearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725) — MetricFrame and `plot_roc_curve_by_group` disagree on how to handle missing sensitive feature values. One raises `ValueError`, the other silently drops rows. This is a real-time argument about whether fairness tools should be strict or permissive — and it's unresolved.

```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame, plot_roc_curve_by_group
# See the missing-value disagreement in Issue #1725
```

**Podcast angle:** When the same toolkit has two different behaviors for missing data — one strict, one silent — which behavior should "fairness" enforce? Does a strict `ValueError` protect users from invisible bias, or does it block legitimate analyses where missing data is the norm?

---

### 5. [Aequitas — DSG (Data & Society Project)](https://github.com/dssg/aequitas) — *Bias Auditing & Fair ML Toolkit*

| | |
|---|---|
| **Stars** | 773 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | DSG (Data & Society Project), University of Toronto |

**What it does:** A bias auditing and fairness toolkit designed for machine learning practitioners and domain experts. Provides group fairness metrics, bipartite ranking metrics, and intersectional analysis. Includes built-in datasets (Compas, Adult, etc.) and Jupyter notebook tutorials. Version 1.0.0 introduced "Aequitas Flow" — a streamlined experiment framework for bias mitigation.

**Why it matters for the podcast:** Aequitas was built from the ground up for **audit contexts** — not just model evaluation, but end-to-end fairness auditing with documentation and reporting. Its design assumes the auditor is a *practitioner* who needs to produce evidence, not just a researcher who needs a metric. This raises a distinct question: **should fairness tools be designed for auditors (evidence-grade) or for researchers (experiment-grade)?**

**Key feature:** The `get_disparity_predefined_group()` function allows auditors to test specific hypotheses about disparities, but as [Issue #86](https://github.com/dssg/aequitas/issues/86) shows, even core functions can break with unexpected errors — raising questions about reliability in production audits.

```bash
pip install aequitas
from aequitas.group import Group
# Run group fairness audits built for evidence production
```

**Podcast angle:** Aequitas bridges the gap between academic fairness research and real-world auditing. But when its own documentation has open gaps ([Issue #201](https://github.com/dssg/aequitas/issues/201) — "Add a readme or page on the existing metrics of fairness"), it mirrors the same documentation crisis seen in AIF360.

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Stars | Focus | Best For |
|---|---|---|---|
| **AIF360** | 2,866 ⭐ | Comprehensive metric library + mitigation | Understanding how fairness is defined in production/government contexts; **live debate in Issue #528** |
| **AIBF_API** | 195 ⭐ | Real-time bias detection in hiring ATS | The hiring-pipeline use case; post-hoc flagging vs. pre-decision interception |
| **Fair-Code** | 47 ⭐ | 7 full bias audits + dataset profiler | Domain-specific audits (COMPAS, healthcare, tenant screening); **live debates in Issues #672, #654** |
| **FairLearn** | 2,286 ⭐ | Fairness assessment + mitigation | Practitioners in Microsoft/MLOps ecosystems; **live debate in Issue #1725** |
| **Aequitas** | 773 ⭐ | Bias auditing + evidence-grade reporting | Auditors who need to produce documentation, not just metrics |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [Aequitas documentation](https://dssg.github.io/aequitas/) — DSG's audit toolkit docs
- [Fair-Code website](https://www.thefaircode.xyz) — Live audits and explainers
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainasan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

## 🎙️ How These Projects Connect to the Podcast

Each episode zooms in on a **specific ongoing disagreement** from an open issue thread:

1. **The Missing-Value Contract Debate** (FairLearn #1725) — Should `MetricFrame` and `plot_roc_curve_by_group` handle missing sensitive features the same way? What does "strict" even mean in a fairness tool?

2. **The Documentation Accuracy Debate** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition?

3. **The SHAP Measurement Problem** (Fair-Code #672) — Does the choice of denominator (top-5 features vs. all features) change the story of racial bias? Who gets to decide what number tells the story of discrimination?

4. **The Counterfactual Fairness Reversal** (Fair-Code #654) — A synthetic lending audit's claimed racial disparity flips direction when actually reproduced. What does it mean when the "expected" outcome contradicts the real one?

5. **The Audit-Grade vs. Research-Grade Divide** (Aequitas #201, #86) — Should fairness tools be built for producing court-ready evidence, or for exploratory research? Can one tool serve both?

6. **The Enterprise-vs-Academic Tension** (Infosys RAI) — When fairness is one module in a commercial responsible-AI suite, does it get the same depth as standalone academic toolkits? Who decides what "fair enough" means for a product that also sells safety and security?

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*