# 📚 Open-Source Fairness Projects — Curated for the Podcast

Three active, impactful projects where the theory of fairness meets the practice of auditing. Each one represents a different philosophy: comprehensive toolkit, real-world audit pipeline, and community-driven evidence.

---

## 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)

| | |
|---|---|
| **Stars** | ⭐ 2,866 |
| **Language** | Python (also R) |
| **License** | Apache-2.0 |
| **Last updated** | September 2026 (actively maintained) |
| **Forks** | 914 |

**What it is:** The canonical open-source fairness toolkit. A comprehensive library of 10+ fairness metrics (demographic parity, equalized odds, predictive parity, calibration, etc.) and 15+ bias-mitigation algorithms spanning pre-processing, in-processing, and post-processing. Developed at IBM Research and now stewarded by the LF AI & Data Foundation.

**Why it matters for the podcast:** AIF360 is the tool that ships in tutorials, gets cited in papers, and underpins enterprise fairness pipelines. When its documentation is wrong, the error propagates everywhere. Its `average_odds_difference` metric has been mislabeled for months — see [Issue #528](https://github.com/Trusted-AI/AIF360/issues/528). And its `Empirical Differential Fairness` metric only returns a single scalar, making intersectional analysis impossible — see [Issue #558](https://github.com/Trusted-AI/AIF360/issues/558).

**Podcast angle:** *The gold standard — but who audits the auditors?*

**Key algorithms:**
- Optimized Preprocessing (Calmon et al., 2017)
- Disparate Impact Remover (Feldman et al., 2015)
- Equalized Odds Postprocessing (Hardt et al., 2016)
- Reweighing (Kamiran & Calders, 2012)
- Adversarial Debiasing (Zhang et al., 2018)
- Exponentiated Gradient Reduction (Agarwal et al., 2018)

**Key metrics:**
- Demographic Parity Difference
- Equalized Odds Difference  
- Predictive Parity Difference
- Average Odds Difference ← *the controversial one*
- Generalized Entropy Index
- Differential Fairness

🔗 **Docs:** [aif360.readthedocs.io](https://aif360.readthedocs.io/en/latest/)  
🔗 **Interactive:** [aif360.res.ibm.com](https://aif360.res.ibm.com/)  
🔗 **Slack:** [aif360.slack.com](https://join.slack.com/t/aif360/shared_invite/zt-5hfvuafo-X0~g6tgJQ~7tIAT~S294TQ)

---

## 2. [Fair-Code (by Yash Kewlani)](https://github.com/yakew7/Fair-Code)

| | |
|---|---|
| **Stars** | ⭐ 47 |
| **Language** | Python / HTML |
| **License** | MIT |
| **Last updated** | September 2026 |

**What it is:** Seven complete, reproducible fairness audits across high-stakes domains: criminal justice (COMPAS), hiring, lending, healthcare, welfare eligibility, and tenant screening. Each audit ships as runnable scripts (`unfair.py` / `fair.py`) plus Jupyter notebooks. Includes an Open Dataset Profiler (web + CLI), a benchmark harness with 5 mitigation strategies (S0–S4), 6 fairness metrics with bootstrap CIs and permutation tests, and 61 plain-language explainers.

**Why it matters for the podcast:** Fair-Code is the most transparently documented fairness project we found. Every audit follows the same pipeline: train a biased model → measure the fairness gap → identify proxy variables → remove them → retrain → measure again. The results are publishable-grade. The healthcare focus is distinctive — three of seven audits examine bias in medical AI, where the consequences are clinical rather than financial. The tenant screening audit delivers a devastating finding: removing race and all 12 proxy variables only cuts the fairness gap from 6.68% to 5.16%, and it stays statistically significant — because the label itself (re-arrest) is a policed quantity.

**Podcast angle:** *What happens when the math itself says "you can't have it all"?*

**Live debates:**
- [Issue #672 — The SHAP Measurement Problem](https://github.com/yakew7/Fair-Code/issues/672): How should you report race's influence? The answer depends on a denominator choice (top-5 features vs. all features), which is a rhetorical decision disguised as a number.
- [Issue #665 — The Impossibility Triangle](https://github.com/yakew7/Fair-Code/issues/665): When base rates differ, you can't satisfy equalized odds and predictive parity simultaneously.

```bash
git clone https://github.com/yakew7/Fair-Code.git
cd Fair-Code
pip install -r requirements.txt
python COMPAS/unfair.py   # see the bias
python COMPAS/fair.py     # see the fix
```

---

## 3. [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)

| | |
|---|---|
| **Stars** | ⭐ 2,286 |
| **Language** | Python |
| **License** | MIT |
| **Last updated** | September 2026 (actively maintained) |
| **Forks** | 516 |

**What it is:** A Python package to assess and improve fairness of machine learning models. Contains mitigation algorithms (Exponentiated Gradient, Grid Search, ThresholdOptimizer) and metrics for model assessment. Designed with scikit-learn integration for MLOps pipelines. Hosted by the Python Software Foundation.

**Why it matters for the podcast:** FairLearn is the practitioner's choice — built by Microsoft, integrated with scikit-learn, and designed for production workflows. But its flagship `MetricFrame` tool only works with metrics that have the signature `metric(y_true, y_pred)`, which excludes dataset-only metrics, streaming metrics, and metrics from other domains like contextual bandits. Issue #756 — open since April 2021 with 74 comments — reveals a deep divide: should the tool be generalized (accepting flexible sample parameters) or kept simple (building new classes for new use cases)?

**Podcast angle:** *Who does the tool serve — the practitioner who wants simplicity, or the researcher who needs generality?*

**Live debate:**
- [Issue #1725 — MetricFrame and missing sensitive feature values](https://github.com/fairlearn/fairlearn/issues/1725): MetricFrame and `plot_roc_curve_by_group` disagree on how to handle missing sensitive feature values. This isn't just a bug; it's a philosophical question about what a fairness tool should do when the data it needs to audit is incomplete.

**Other active issues:**
- [#1419](https://github.com/fairlearn/fairlearn/issues/1419) — Adding statistical property/integrity tests for the adversarial mitigation module (5 comments)
- [#1417](https://github.com/fairlearn/fairlearn/issues/1417) — Replace PyTorch and TensorFlow test mocks with actual instances (4 comments)

```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame
# The tool that's at the center of the API design debate
```

---

## 📊 Comparison Matrix

| | AIF360 | Fair-Code | FairLearn |
|---|---|---|---|
| **Stars** | 2,866 | 47 | 2,286 |
| **Philosophy** | Comprehensive toolkit | Transparent audit pipeline | Practitioner-friendly API |
| **Maintainer** | IBM / LF AI | Solo + community | Microsoft / PSF |
| **Best for** | Understanding formal metric definitions | Domain-specific audits (COMPAS, healthcare, hiring) | Production MLOps integration |
| **Live debate** | #528 — What does "zero" mean? | #672 — Does the denominator change the story? | #1725 — What serves who? |
| **License** | Apache-2.0 | MIT | MIT |

---

## 🔗 Essential External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation (see Issue #528 for known errors)
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [Fair-Code explainers](https://www.thefaircode.xyz) — 61 plain-language fairness explainers
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The average_odds_difference documentation bug (open since April 2024)
- [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Extend Empirical Differential Fairness for intersectional analysis (open since January 2026)
- [FairLearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725) — MetricFrame and missing sensitive feature values (open since September 2026)
- [Fair-Code Issue #672](https://github.com/yakew7/Fair-Code/issues/672) — The SHAP Measurement Problem
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainasan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

## 🎙️ How These Projects Connect to the Podcast

Each episode zooms in on a **real, unresolved controversy** from an open issue thread:

1. **The Average-Odds Documentation Bug** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition? After 17 months, a volunteer offered a fix — but no maintainer has merged it.
2. **The Intersectional Analysis Gap** (AIF360 #558) — Should fairness tools return a single scalar, or should they break down discrimination by specific group combinations? A volunteer offered to implement the enhancement.
3. **The Missing-Value Contract** (FairLearn #1725) — Should fairness tools be strict or silent when sensitive data is missing? The way you answer this question reveals what you think a fairness tool is for.
4. **The SHAP Measurement Problem** (Fair-Code #672) — Does the choice of denominator change the story of racial bias?
5. **The Impossibility Triangle** (Fair-Code #665) — When two metrics give contradictory answers, whose rights should the metric protect?

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*