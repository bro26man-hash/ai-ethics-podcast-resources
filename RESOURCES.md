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
| **Last updated** | September 2026 (very active) |
| **Forks** | 44 |

**What it is:** A one-person research operation that runs end-to-end bias audits on seven real-world domains: COMPAS criminal justice, hiring, lending, insurance denial, welfare eligibility, healthcare readmission, and tenant screening. Each audit ships as `unfair.py` (biased model) and `fair.py` (mitigated model) with before/after fairness gap measurements.

**Why it matters for the podcast:** Fair-Code is fairness auditing as *journalism*. It doesn't just implement metrics — it tells stories with data. The tenant screening audit is a masterclass in how removing "race" from a model changes almost nothing when the label itself (re-arrest) is policed along racial lines.

**Podcast angle:** *Fairness auditing as investigative reporting — one auditor, seven domains, zero shortcuts.*

**Audit pipeline:**
```
Train biased model → Measure fairness gap → Identify proxies → Remove protected attrs + proxies → Retrain → Measure again
```

**Key feature:** 61 plain-language explainers covering everything from "What is a Proxy Variable?" to "What Is Simpson's Paradox in Fairness Audits?"

🔗 **Live website:** [thefaircode.xyz](https://www.thefaircode.xyz)  
🔗 **Open Dataset Profiler:** Drop in a CSV, get a demographic representation audit in your browser

---

## 3. [Aequitas](https://github.com/dssg/aequitas)

| | |
|---|---|
| **Stars** | ⭐ 773 |
| **Language** | Python |
| **License** | MIT |
| **Last updated** | September 2026 (actively maintained) |
| **Forks** | 125 |

**What it is:** A bias auditing and "correction" toolkit from the Data & Society Research Institute (University of Chicago). Unlike AIF360's comprehensive approach, Aequitas is designed specifically for the **audit → correction → experiment** workflow, making it the most practitioner-friendly toolkit for non-technical audiences.

**Why it matters for the podcast:** Aequitas is built by social scientists and engineers who intentionally design for policymakers, not just data scientists. Its `Audit` class produces group-level fairness reports with built-in visualizations, and its `Aequitas Flow` extension adds bias mitigation experiments. It's the bridge between academic fairness research and real-world governance.

**Podcast angle:** *The people's toolkit — fairness auditing designed for the communities being audited, not just the engineers building the models.*

**Key features:**
- **Audit module:** Confusion-matrix-based fairness metrics per group (TPR, FPR, PPV, NPV) with disparity plots
- **Flow experiments:** Pre-processing (Data Repairer, Prevalence Sampling), in-processing (FairGBM, Fairlearn), and post-processing (Group Threshold, Balanced Group Threshold)
- **Visualization:** Built-in summary and disparity plots — no matplotlib expertise required
- **Datasets:** BankAccountFraud and FolkTables included for reproducible research
- **Extensibility:** User-implemented methods with intuitive interfaces

**Key fairness concepts covered:**
- Predictive Equality (equal FPR across groups)
- Demographic Parity (equal selection rates)
- Equalized Odds (equal TPR and FPR)
- Calibration (equal precision across groups)

🔗 **Docs:** [dssg.github.io/aequitas](https://dssg.github.io/aequitas/)  
🔗 **Colab tutorials:** [Notebooks](https://github.com/dssg/aequitas/tree/notebooks)  
🔗 **Project site:** [dsapp.uchicago.edu/aequitas](http://dsapp.uchicago.edu/aequitas/)

---

## 4. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)

| | |
|---|---|
| **Stars** | ⭐ 310 |
| **Language** | Python |
| **License** | MIT |
| **Last updated** | September 2026 (actively maintained) |
| **Forks** | 82 |

**What it is:** An enterprise-grade, modular toolkit from Infosys that covers fairness, safety, privacy, security, explainability, and hallucination detection — with a strong focus on **LLMs** as well as traditional ML. Unlike the academic tone of AIF360, this is fairness tooling built for corporate production pipelines.

**Why it matters for the podcast:** This is what "responsible AI" looks like when a 100,000-employee consultancy is shipping it to clients. It reveals the tensions between corporate needs and ethical ideals — and shows how fairness gets operationalized (and sometimes diluted) in enterprise contexts. The Fairness & Bias module implements Statistical Parity Difference, Disparate Impact Ratio, Four-Fifths Rule, Cohen's D, Equalized Odds, and Re-weighing.

**Podcast angle:** *Fairness as a service — when bias auditing becomes a product, who sets the standards?*

**Key modules:**
- **Fairness & Bias API:** For both LLM prompts/responses and traditional ML models
- **Moderation Layer:** Safety, privacy, explainability, and hallucination detection
- **Explainability:** SHAP (global) and LIME (local) for model interpretation
- **Security:** Adversarial attack simulation and defense recommendations
- **Red Teaming:** PAIR and TAP techniques for LLM robusteness

🔗 **Installation:** [README](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit/blob/master/README.md)  
🔗 **Features doc:** [2.2.1 docx](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit/blob/master/Features%20and%20Endpoints-2.2.1.docx)

---

## How to Use This Catalog

| If you're a… | Start with | Then explore |
|---|---|---|
| **Practitioner** building a fairness pipeline | AIF360 (comprehensive) | Aequitas (audit → correction workflow) |
| **Journalist / documentarian** | Fair-Code (real audits, real stories) | AIF360 issue #528 (the documentation debate) |
| **Policy researcher** | Aequitas (designed for non-experts) | Infosys toolkit (enterprise operationalization) |
| **Curious listener** | Fair-Code's [explainers](https://www.thefaircode.xyz) | This repo's `DEBATES.md` |

---

*To add a project: Open a PR or open an issue tagged `debate-nomination`. Include: repo link, stars, license, what fairness problem it solves, and why a podcast listener should care.*