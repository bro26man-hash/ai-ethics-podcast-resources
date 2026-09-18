# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of **active open-source projects** related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects

### 1. [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360) — *Comprehensive Fairness Metric Toolkit*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
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

### 2. [Fair-Code](https://github.com/yakew7/Fair-Code) — *Algorithmic Bias Detection & Mitigation (Research Audits)*

| | |
|---|---|
| **Stars** | ~47 ⭐ |
| **Language** | Python, HTML, Jupyter Notebook |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** Seven complete bias audits across criminal justice (COMPAS), hiring, lending, insurance denial, welfare eligibility, healthcare readmission, and tenant screening. Each audit ships as a reproducible `unfair.py` / `fair.py` pair plus Jupyter notebooks.

**Why it matters for the podcast:** Fair-Code is the most essayistic project in the fairness space — its 61 explainers cover everything from "What Is a Proxy Variable?" to "Why Fairness Metrics Conflict." It's also the site of **featured debates** in `DEBATES.md`.

**Podcast angle:** The ProPublica vs. Northpointe COMPAS dispute is the showpiece — two parties both mathematically correct because they measured different fairness criteria.

**Run it locally:**
```bash
# Clone and run individual audits
cd audits/compas
python unfair.py  # Baseline (unfair) model
python fair.py    # Fairness-mitigated model
```

---

### 3. [EqualityML](https://github.com/EqualityAI/EqualityML) — *Evidence-Based Tools for Ending Algorithmic Bias*

| | |
|---|---|
| **Stars** | ~35 ⭐ |
| **Language** | Jupyter Notebook |
| **License** | MIT |
| **Last Updated** | Active |
| **Maintainer** | EqualityAI community |

**What it does:** Evidence-based tools and community collaboration to end algorithmic bias, one data scientist at a time. Provides interfacing dashboards and nbval test notebooks to measure and compare bias in ML models.

**Why it matters for the podcast:** EqualityML represents the **community-driven** model of fairness tooling — not backed by a big-tech org, but maintained by a collective of practitioners. Its "one data scientist at a time" framing highlights the grassroots nature of the fairness movement. The question is whether community-maintained tools can achieve the same rigor and adoption as IBM or Microsoft toolkits.

**Podcast angle:** Can a community-owned fairness toolkit rival corporate-maintained ones? What do we gain in independence and what do we lose in scale?

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Focus | Maturity | Best For |
|---|---|---|---|
| **AIF360** | Comprehensive metric library + mitigation | Industry-standard, IBM-maintained | Understanding how fairness is defined in production/government contexts |
| **Fair-Code** | Research audits + explainers | Production-quality audits, active development | Deep dives on specific domains (healthcare, justice, lending) |
| **EqualityML** | Community-driven bias measurement | Grassroots, notebook-based | Hands-on fairness experimentation and learning |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [Fair-Code live site](https://www.thefaircode.xyz) — Hosted explainers and interactive profiler
- [AIF360 Issue #528 — average_odds_difference controversy](https://github.com/Trusted-AI/AIF360/issues/528) — The debate detailed in `DEBATES.md`
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainasan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*
