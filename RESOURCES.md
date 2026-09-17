# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of active open-source projects related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects

### 1. [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360) — *Comprehensive Fairness Metric Toolkit*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Forks** | 914 |
| **Language** | Python (also R) |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |
| **Maintainer** | IBM Research (Trusted-AI org) |

**What it does:** A comprehensive set of fairness metrics for datasets and machine learning models, with explanations for each metric and algorithms to mitigate bias. Covers over 20 fairness metrics across multiple categories: group fairness (demographic parity, equalized odds, predictive parity), individual fairness, and causal fairness. Includes bias mitigation algorithms for pre-processing, in-processing, and post-processing.

**Why it matters for the podcast:** AIF360 is the most widely deployed fairness toolkit in production ML pipelines. Its metric definitions are the de facto standard for fairness auditing in enterprise and government contexts. When the tool itself mislabels a metric — as in **Issue #528** (see `DEBATES.md`) — the stakes are not academic: courts and regulators reference these definitions.

**Key feature:** The `ClassificationMetric` class provides a unified API for computing fairness metrics, but as Issue #528 reveals, the documentation for `average_odds_difference` misrepresents what the metric actually measures.

**Run it locally:**
```bash
pip install aif360
from aif360.metrics import ClassificationMetric
# See metric definitions and use examples in the docs
```

**Podcast angle:** When the most-cited fairness toolkit ships with a documentation error about what "zero" means, who catches it — and who decides what the number means?

---

### 2. [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn) — *Fairness Assessment & Mitigation*

| | |
|---|---|
| **Stars** | 2,286 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Microsoft (fairlearn org) |

**What it does:** A Python package to assess and improve the fairness of machine learning models. Supports key fairness metrics (demographic parity, equalized odds, predictive parity, etc.) and mitigation algorithms (thresholding, re-weighting, adversarial debiasing). Designed for integration with scikit-learn workflows.

**Why it matters for the podcast:** FairLearn is Microsoft's answer to AIF360 — same core metrics, different design philosophy. Where AIF360 is an IBM Research project with academic roots, FairLearn is built for practical ML pipelines inside Microsoft's ecosystem. The two toolkits sometimes define and compute the same metrics slightly differently, raising the question: **if two mainstream fairness tools disagree on a metric's definition, which one is "right"?**

**Podcast angle:** The AIF360 vs. FairLearn divergence — when two big-tech fairness toolkits encode different mathematical choices, practitioners and regulators have no single authoritative reference.

---

### 3. [Responsibly / ResponsiblyAI](https://github.com/ResponsiblyAI/responsibly) — *Bias Auditing & Mitigation Pipeline*

| | |
|---|---|
| **Stars** | 101 ⭐ |
| **Forks** | 23 |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** A toolkit for auditing and mitigating bias and fairness in machine learning systems. Provides directional bias detection, fairness metrics, and mitigation algorithms with a particular focus on NLP models. Aligned with the book *Fairness and Machine Learning* by Barocas, Hardt, and Narayanan.

**Why it matters for the podcast:** Responsibly occupies a middle ground — more structured than research-audit projects, more open than governance platforms. It's a case study in how fairness tooling evolves from research prototypes to maintainable packages. Its NLP focus also highlights a gap: most fairness tools are built for tabular data, but NLP models are increasingly used in high-stakes decisions (hiring screening, loan applications, content moderation).

**Podcast angle:** How do fairness toolkits survive maintainer burnout? What does it mean when a team shifts from "research artifacts" to "maintained packages"? And why is NLP fairness so much harder than tabular fairness?

---

### 4. [Fair-Code](https://github.com/yakew7/Fair-Code) — *Algorithmic Bias Detection & Mitigation (Research Audits)*

| | |
|---|---|
| **Stars** | ~47 ⭐ |
| **Forks** | ~45 |
| **Language** | Python, HTML, Jupyter Notebook |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** Seven complete bias audits across criminal justice (COMPAS), hiring, lending, insurance denial, welfare eligibility, healthcare readmission, and tenant screening. Each audit ships as a reproducible `unfair.py` / `fair.py` pair plus Jupyter notebooks.

**Why it matters for the podcast:** Fair-Code is the most essayistic project in the fairness space — its 61 explainers cover everything from "What Is a Proxy Variable?" to "Why Fairness Metrics Conflict." It's also the site of **featured debates** in `DEBATES.md`.

**Podcast angle:** The ProPublica vs. Northpointe COMPAS dispute is the showpiece — two parties both mathematically correct because they measured different fairness criteria.

---

### 5. [AWS SageMaker Clarify](https://github.com/aws/amazon-sagemaker-clarify) — *Fairness-Aware ML on AWS*

| | |
|---|---|
| **Stars** | 75 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | May 2026 |
| **Maintainer** | AWS |

**What it does:** Fairness-aware machine learning. Bias detection and mitigation for datasets and models, integrated into the SageMaker ML pipeline.

**Why it matters for the podcast:** Clarify represents the "fairness-as-a-service" model — bias auditing embedded in a commercial cloud platform. This raises the question: can fairness tools be both open-source and cloud-vendor-affiliated? Who controls the narrative when the auditor is also the platform provider?

**Podcast angle:** The cloud-vendor fairness tool dilemma — audit independence when your auditor is also your competitors' infrastructure.

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Focus | Maturity | Best For |
|---|---|---|---|
| **AIF360** | Comprehensive metric library + mitigation | Industry-standard, IBM-maintained | Understanding how fairness is defined in production/government contexts |
| **FairLearn** | Fairness assessment + mitigation | Microsoft-maintained, scikit-learn integration | Practitioners working in Microsoft/MLOps ecosystems |
| **Responsibly** | Bias auditing + mitigation pipeline | Active development, growing community | Practical auditing with directional bias detection and NLP focus |
| **Fair-Code** | Research audits + explainers | Production-quality audits, active development | Deep dives on specific domains (healthcare, justice, lending) |
| **SageMaker Clarify** | Cloud-integrated fairness-aware ML | AWS-maintained, commercial integration | Fairness auditing within AWS ML pipelines |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [Responsibly documentation](https://docs.responsibly.ai) — Toolkit docs and guides
- [Fair-Code live site](https://www.thefaircode.xyz) — Hosted explainers and interactive profiler
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainasan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*