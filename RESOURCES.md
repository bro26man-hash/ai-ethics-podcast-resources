# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of active open-source projects related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects

### 1. [Fair-Code](https://github.com/yakew7/Fair-Code) — *Algorithmic Bias Detection & Mitigation*

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Forks** | 45 |
| **Language** | Python, HTML, Jupyter Notebook |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** Seven complete bias audits across criminal justice (COMPAS), hiring, lending, insurance denial, welfare eligibility, healthcare readmission, and tenant screening. Each audit ships as a reproducible `unfair.py` / `fair.py` pair plus Jupyter notebooks walking through the full pipeline: train a biased model → measure the fairness gap → identify proxy variables → engineer a fair model → measure again.

**Why it matters for the podcast:** Fair-Code is the most essayistic project in the fairness space — its [61 explainers](https://github.com/yakew7/Fair-Code/tree/main/explainers) cover everything from "What Is a Proxy Variable?" to "Why Fairness Metrics Conflict." It's also the site of the **featured debates** in `DEBATES.md`.

**Key feature:** The [Open Dataset Profiler](https://github.com/yakew7/Fair-Code#open-dataset-profiler) audits datasets themselves for demographic representation — a different lens from model-level fairness.

**Run it locally:**
```bash
pip install -r requirements.txt
python COMPAS/unfair.py   # see the bias
python COMPAS/fair.py     # see the fix
```

**Podcast angle:** The ProPublica vs. NorthpointeCOMPAS dispute is the showpiece — two parties both mathematically correct because they measured different fairness criteria. Episode material writes itself.

---

### 2. [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360) — *Comprehensive Fairness Metric Toolkit*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |
| **Maintainer** | IBM Research (Trusted-AI org) |

**What it does:** A comprehensive set of fairness metrics for datasets and machine learning models, with explanations for each metric and algorithms to mitigate bias. Covers over 20 fairness metrics across multiple categories: group fairness (demographic parity, equalized odds, predictive parity), individual fairness, and causal fairness. Includes bias mitigation algorithms for pre-processing, in-processing, and post-processing.

**Why it matters for the podcast:** AIF360 is the most widely deployed fairness toolkit in production ML pipelines. Its metric definitions are the de facto standard for fairness auditing in enterprise and government contexts. When the tool itself mislabels a metric — as in the **featured debate** in `DEBATES.md` — the stakes are not academic: courts and regulators reference these definitions.

**Key feature:** The `ClassificationMetric` class provides a unified API for computing fairness metrics, but asIssue #528 reveals, the documentation for `average_odds_difference` misrepresents what the metric actually measures.

**Run it locally:**
```bash
pip install aif360
from aif360.metrics import ClassificationMetric
# See metric definitions and use examples in the docs
```

**Podcast angle:** When the most-cited fairness toolkit ships with a documentation error about what "zero" means, who catches it — and who decides what the number means?

---

### 3. [Responsibly / ResponsiblyAI](https://github.com/ResponsiblyAI/responsibly) — *Bias Auditing & Mitigation Toolkit*

| | |
|---|---|
| **Stars** | 101 ⭐ |
| **Language** | Python |
| **License** | — |
| **Last Updated** | September 2026 |

**What it does:** A toolkit for auditing and mitigating bias and fairness in machine learning systems. Provides directional bias detection, fairness metrics, and mitigation algorithms with a focus on practical deployment.

**Why it matters for the podcast:** Responsibly occupies a middle ground — more structured than Fair-Code's research audits, more open than FairMind's governance alpha. It's a case study in how fairness tooling evolves from research prototypes to maintainable packages.

**Podcast angle:** How do fairness toolkits survive maintainer burnout? What does it mean when a team shifts from "research artifacts" to "maintained packages"?

---

### 4. [AWS SageMaker Clarify](https://github.com/aws/amazon-sagemaker-clarify) — *Fairness-Aware ML on AWS*

| | |
|---|---|
| **Stars** | 75 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | May 2026 |
| **Maintainer** | AWS |

**What it does:** Fairness-aware machine learning. Bias detection and mitigation for datasets and models, integrated into the SageMaker ML pipeline. Supports common fairness metrics and bias mitigation techniques.

**Why it matters for the podcast:** Clarify represents the "fairness-as-a-service" model — bias auditing embedded in a commercial cloud platform. This raises the question: can fairness tools be both open-source and cloud-vendor-affiliated? Who controls the narrative when the auditor is also the platform provider?

**Podcast angle:** The cloud-vendor fairness tool dilemma — audit independence when your auditor is also your competitors' infrastructure.

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Focus | Maturity | Best For |
|---|---|---|---|
| **Fair-Code** | Research audits + explainers | Production-quality audits, active development | Deep dives on specific domains (healthcare, justice, lending) |
| **AIF360** | Comprehensive metric library + mitigation | Industry-standard, IBM-maintained | Understanding how fairness is defined in production/government contexts |
| **Responsibly** | Bias auditing + mitigation pipeline | Active development, growing community | Practical auditing with directional bias detection |
| **SageMaker Clarify** | Cloud-integrated fairness-aware ML | AWS-maintained, commercial integration | Fairness auditing within AWS ML pipelines |
| **Algorithmic-Fairness-Toolkit** | Metric comparison + trade-off analysis | Functional toolkit with Streamlit dashboard | Understanding what you lose when you gain fairness |
| **FairMind** | Governance & compliance assurance | Internal alpha, building evidence infrastructure | Enterprise audit trails, regulatory mapping |

---

## 📖 Essential Explainers (from Fair-Code)

These 61 plain-language explainers are among the best free educational resources on algorithmic fairness:

**Core concepts:**
- [What Is a Proxy Variable?](https://github.com/yakew7/Fair-Code/blob/main/explainers/proxy-variables.md) — Why removing race isn't enough
- [Why Fairness Metrics Conflict](https://github.com/yakew7/Fair-Code/blob/main/explainers/fairness-metric-conflicts.md) — The mathematical impossibility theorem
- [What Is Counterfactual Fairness?](https://github.com/yakew7/Fair-Code/blob/main/explainers/counterfactual-fairness.md) — The causal approach to fairness
- [What Is Demographic Parity?](https://github.com/yakew7/Fair-Code/blob/main/explainers/demographic-parity.md) — The equal-outcome-rate metric
- [What Is Equalized Odds?](https://github.com/yakew7/Fair-Code/blob/main/explainers/equalized-odds.md) — The equal-error-rate metric
- [What Is Predictive Parity?](https://github.com/yakew7/Fair-Code/blob/main/explainers/predictive-parity.md) — The equal-reliability metric

**Healthcare focus:**
- [Why Accuracy Is Not Enough in Healthcare AI](https://github.com/yakew7/Fair-Code/blob/main/explainers/accuracy-not-enough-healthcare-ai.md)
- [Race Correction in Clinical Algorithms](https://github.com/yakew7/Fair-Code/blob/main/explainers/race-correction-clinical-algorithms.md)
- [The Obermeyer Case: When Cost Becomes a Proxy for Health Need](https://github.com/yakew7/Fair-Code/blob/main/explainers/obermeyer-cost-proxy.md)

**Advanced topics:**
- [What Is Intersectional Bias?](https://github.com/yakew7/Fair-Code/blob/main/explainers/intersectional-bias.md)
- [What Is Differential Privacy (and Its Tension With Fairness)?](https://github.com/yakew7/Fair-Code/blob/main/explainers/differential-privacy.md)
- [What Is Simpson's Paradox in Fairness Audits?](https://github.com/yakew7/Fair-Code/blob/main/explainers/simpsons-paradox.md)

## 🔗 External Resources

- [Fair-Code live site](https://www.thefaircode.xyz) — Hosted explainers and interactive profiler
- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainathan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*