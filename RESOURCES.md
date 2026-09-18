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

**Why it matters for the podcast:** AIF360 is the tool that ships in tutorials, gets cited in papers, and underpins enterprise fairness pipelines. When its documentation is wrong, the error propagates everywhere. Its `average_odds_difference` metric has been mislabeled for months — see [Issue #528](https://github.com/Trusted-AI/AIF360/issues/528).

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
| **Last updated** | September 2026 (very active) |
| **Forks** | 44 |

**What it is:** A one-person research operation that runs end-to-end bias audits on seven real-world domains: COMPAS criminal justice, hiring, lending, insurance denial, welfare eligibility, healthcare readmission, and tenant screening. Each audit ships as `unfair.py` (biased model) and `fair.py` (mitigated model) with before/after fairness gap measurements.

**Why it matters for the podcast:** Fair-Code is fairness auditing as *journalism*. It doesn't just implement metrics — it tells stories with data. The tenant screening audit is a masterclass in how removing "race" from a model changes almost nothing when the label itself (re-arrest) is policed along racial lines.

**Podcast angle:** *Fairness auditing as investigative reporting — one auditor, seven domains, zero中东girls.*

**Audit pipeline:**
```
Train biased model → Measure fairness gap → Identify proxies → Remove protected attrs + proxies → Retrain → Measure again
```

**Key feature:** 61 plain-language explainers covering everything from "What is a Proxy Variable?" to "What Is Simpson's Paradox in Fairness Audits?"

🔗 **Live website:** [thefaircode.xyz](https://www.thefaircode.xyz)  
🔗 **Open Dataset Profiler:** Drop in a CSV, get a demographic representation audit in your browser

---

## 3. [EqualityML (by Equality AI)](https://github.com/EqualityAI/EqualityML)

| | |
|---|---|
| **Stars** | ⭐ 35 |
| **Language** | Jupyter Notebook / Python (also R on CRAN) |
| **License** | Apache-2.0 |
| **Last updated** | Active community contributions |
| **Forks** | 2 |

**What it is:** A community-driven, evidence-based toolkit from Equality AI (a public-benefit corporation) that unifies fairness metrics and bias mitigation into a single `FAIR` API. 12 fairness metrics and 4 bias mitigation methods, with a decision-tree questionnaire to help practitioners choose the right metric for their use case.

**Why it matters for the podcast:** EqualityML asks a question that AIF360 doesn't: *"How do you choose which fairness metric to use?"* Their [Fairness Metric Selection Questionnaire & Tree](https://github.com/EqualityAI/EqualityML/blob/main/Equality%20AI%20Fairness%20Metric%20Selection%20Questionnaire%20%26%20Tree.pdf) is itself a contribution to the methodology debate — literally a flowchart for deciding what "fair" means in your context.

**Podcast angle:** *The metric selection problem — because between demographic parity and equalized odds, there are 12 options and no consensus on which to pick.*

**Available metrics:**
- Statistical Parity, Conditional Statistical Parity
- Negative Predictive Parity, Predictive Parity
- Equal Opportunity, Equalized Odds
- Balance for Positive/Negative Class
- Predictive Equality, Well Calibration
- Conditional Use Accuracy, Overall Balance

**Available mitigations:**
- Resampling, Reweighting
- Disparate Impact Remover, Correlation Remover

🔗 **PyPI:** `pip install equalityml`  
🔗 **Slack:** [equalityai.com/slack](https://equalityai.com/community/#manifesto)

---

## How to Use This Catalog

| If you're a… | Start with | Then explore |
|---|---|---|
| **Practitioner** building a fairness pipeline | AIF360 (comprehensive) | EqualityML (metric selection guide) |
| **Journalist / documentarian** | Fair-Code (real audits, real stories) | AIF360 issue #528 (the documentation debate) |
| **Researcher** | AIF360 (algorithms + metrics) | EqualityML (methodology formalism) |
| **Curious listener** | Fair-Code's [explainers](https://www.thefaircode.xyz) | This repo's `DEBATES.md` |

---

*To add a project: Open a PR or open an issue tagged `debate-nomination`. Include: repo link, stars, license, what fairness problem it solves, and why a podcast listener should care.*