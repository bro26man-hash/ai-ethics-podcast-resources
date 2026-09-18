# 📚 Open-Source Fairness Projects — Curated for the Podcast

Four active, impactful projects where the theory of fairness meets the practice of auditing. Each one represents a different philosophy: comprehensive toolkit, real-world audit pipeline, enterprise-grade assurance, and community-driven API framework.

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

## 2. [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)

| | |
|---|---|
| **Stars** | ⭐ 870 |
| **Language** | Python |
| **License** | MIT |
| **Last updated** | September 2026 (actively maintained) |
| **Forks** | 256 |

**What it is:** A community-driven toolkit from Microsoft for assessing and improving fairness in ML systems. FairLearn centers on the `MetricFrame` object — a unified interface for computing fairness metrics disaggregated by sensitive features. It emphasizes **dependency-aware metrics** that explicitly condition fairness on underlying causal assumptions.

**Why it matters for the podcast:** FairLearn represents the "API philosophy" approach to fairness — the question isn't just *what* to measure, but *how to structure the measurement* so it can't be misused. Its most active debate (Issue #756, 74 comments, open since April 2021) asks whether MetricFrame should support metrics beyond the standard `metric(y_true, y_pred)` signature —Dataset-only metrics, reinforcement learning metrics, and metrics for partial-observation settings like lending. The debate reveals a fundamental tension: should a fairness tool be a **general-purpose framework** or a **specialized instrument**?

**Podcast angle:** *When the API itself becomes a fairness decision — who should a tool serve?*

**Key features:**
- **MetricFrame:** Unified fairness metric computation with group disaggregation
- **Dependency-aware metrics:** Explicit conditioning on causal assumptions
- **Reduction algorithms:** For translating fairness constraints into standard ML objectives
- **Visualization utilities:** ROC curves by group, bar plots for metric comparisons
- **Integration with scikit-learn:** Familiar API for ML practitioners

**Key metrics:**
- Demographic Parity Difference
- Equalized Odds Difference
- Predictive Parity Difference
- Average Odds Difference
- generalized_odds_difference
- Theil Index

**Active issues:**
- [#756 — MetricFrame should support metrics that don't require y_true and y_pred](https://github.com/fairlearn/fairlearn/issues/756): 74 comments, 5+ years old, unresolved. Should MetricFrame be a general-purpose framework or a specialized instrument? Should arguments be keyword-only? Should `metric` become `metrics`? This is the central API design debate of the fairness tooling ecosystem.
- [#1725 — Missing-value contract](https://github.com/fairlearn/fairlearn/issues/1725): Should fairness tools fail loudly, proceed silently, or impute when sensitive data is missing?

```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame
mf = MetricFrame(metrics=accuracy_score, y_true=y_true, y_pred=y_pred, sensitive_features=sf)
print(metric_frame.by_group)
```

🔗 **Docs:** [fairlearn.ai](https://fairlearn.ai/)  
🔗 **Repo:** [github.com/fairlearn/fairlearn](https://github.com/fairlearn/fairlearn)  
🔗 **Issue #756:** [The MetricFrame API design debate](https://github.com/fairlearn/fairlearn/issues/756)

---

## 3. [Aequitas — University of Chicago (Data & Society Project)](https://github.com/dssg/aequitas)

| | |
|---|---|
| **Stars** | ⭐ 773 |
| **Language** | Python |
| **License** | MIT |
| **Last updated** | September 2026 (actively maintained) |
| **Forks** | 125 |

**What it is:** An open-source bias auditing and Fair ML toolkit designed specifically for **data scientists, ML researchers, and policymakers**. Aequitas provides an easy-to-use, transparent tool for auditing predictors of ML models and experimenting with "correcting biased models" using Fair ML methods in binary classification settings. Version 1.0.0 introduced **Aequitas Flow**, a streamlined pipeline for bias audits with mitigation.

**Why it matters for the podcast:** Aequitas bridges the gap between technical rigor and policy relevance. Its documentation explicitly targets policymakers, not just engineers. The COMPAS demo notebook walks through a full criminal justice audit. And its new "Flow" feature asks: can you go from detecting bias to fixing it in a single, reproducible pipeline?

**Podcast angle:** *Can you audit an algorithm and then un-audit it — and who gets to decide what "corrected" means?*

**Key features:**
- **Confusion-matrix-based metrics** per sensitive group (TPR, FPR, PPV, NPV, etc.)
- **Bias audit summaries** with visual disparity plots
- **Fair ML methods**: Pre-processing (Data Repairer, Label Flipping, Prevalence Sampling, Massaging, Correlation Suppression), In-processing (FairGBM, Fairlearn Classifier), Post-processing (Group Threshold, Balanced Group Threshold)
- **Built-in datasets**: BankAccountFraud and FolkTables
- **Hyperparameter optimization** via Optuna integration
- **Reproducibility**: Save all experiment artifacts

**Active issues:**
- [#201 — Add a readme or page on the existing metrics of fairness](https://github.com/dssg/aequitas/issues/201): Even the project itself acknowledges that its metrics need better documentation. A "Good First Issue" assigned to multiple contributors — but still open since July 2024.
- [#209 — Please Release New Versions When Making Breaking Changes](https://github.com/dssg/aequitas/issues/209): A user flagged that breaking changes were shipped without a new version number. Trust isn't just about code — it's about versioning.

```bash
pip install aequitas
from aequitas import Audit
audit = Audit(df)
audit.summary_plot(["tpr", "fpr", "pprev"])
```

🔗 **Docs:** [dssg.github.io/aequitas](https://dssg.github.io/aequitas/)  
🔗 **Tutorial:** [dssg.github.io/fairness_tutorial](https://dssg.github.io/fairness_tutorial/)  
🔗 **Project site:** [dsapp.uchicago.edu/aequitas](http://dsapp.uchicago.edu/aequitas/)

---

## 4. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)

| | |
|---|---|
| **Stars** | ⭐ 310 |
| **Language** | Python |
| **License** | Apache-2.0 |
| **Last updated** | September 2026 (actively maintained) |

**What it is:** A comprehensive enterprise-grade Responsible AI toolkit from Infosys that goes beyond fairness to cover **safety, security, explainability, bias, and hallucination detection**. Designed for production ML pipelines where regulatory compliance is not optional.

**Why it matters for the podcast:** This toolkit represents the "corporate" approach to AI ethics — built by a major IT services company, designed for enterprise clients who need to demonstrate compliance with the EU AI Act, NIST AI RMF, and emerging regulations. It raises a key question: when a corporation builds the fairness tooling, whose definition of "fair" ships by default?

**Podcast angle:** *When Big Tech builds the fairness tools, do they build them for the people who are audited — or for the people who audit?*

**Key features:**
- **Fairness detection** across multiple protected attributes
- **Bias mitigation** algorithms for pre- and post-processing
- **Explainability** module for model transparency
- **Safety & security** checks beyond traditional fairness
- **Hallucination detection** for generative AI systems

🔗 **Repo:** [Infosys/Infosys-Responsible-AI-Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)

---

## 📊 Comparison Matrix

| | AIF360 | FairLearn | Aequitas | Infosys RAI |
|---|---|---|---|---|
| **Stars** | 2,866 | 870 | 773 | 310 |
| **Philosophy** | Comprehensive research toolkit | Community-driven API framework | Policy-friendly audit pipeline | Enterprise compliance platform |
| **Maintainer** | IBM / LF AI & Data | Microsoft / community | UChicago / Data & Society | Infosys |
| **Best for** | Formal metric definitions & research | Structured API & dependency-aware metrics | Policymaker-facing audits & reproducible pipelines | Enterprise regulatory compliance |
| **Live debate** | #528 — What does "zero" mean? | #756 — General-purpose framework or specialized instrument? | #201 — Even the docs need docs | Enterprise whose fairness definition ships by default?
| **License** | Apache-2.0 | MIT | MIT | Apache-2.0 |

---

## 🔗 Essential External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation (see Issue #528 for known errors)
- [FairLearn documentation](https://fairlearn.ai/) — Microsoft's community-driven fairness toolkit docs
- [Aequitas documentation](https://dssg.github.io/aequitas/) — UChicago's policy-friendly audit toolkit docs
- [Aequitas Flow paper (JMLR 2024)](https://arxiv.org/pdf/2405.05809) — "Streamlining Fair ML Experimentation"
- [Aequitas original paper (2018)](https://arxiv.org/pdf/1811.05577.pdf) — "Aequitas: A Bias and Fairness Audit Toolkit"
- [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — The MetricFrame API design debate (74 comments, open since April 2021)
- [FairLearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725) — The missing-value contract
- [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — Enterprise-grade responsible AI
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The average_odds_difference documentation bug (open since April 2024)
- [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Extend Empirical Differential Fairness for intersectional analysis (open since January 2026)
- [Aequitas Issue #201](https://github.com/dssg/aequitas/issues/201) — Add a readme on existing metrics (open since July 2024)
- [Fair-Code project](https://github.com/yakew7/Fair-Code) — Seven complete, reproducible fairness audits
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
3. **The MetricFrame API Design Debate** (FairLearn #756) — Should a fairness tool be a general-purpose framework that handles any kind of metric, or a specialized instrument built only for classification? 74 comments, 5+ years, unresolved. The answer determines who the tool serves: researchers who need flexibility, or practitioners who need simplicity.
4. **The Metrics Governance Gap** (Aequitas #201) — Even Aequitas, the toolkit built for policymakers, acknowledges that its own metrics need better documentation. A "Good First Issue" open since July 2024, assigned to three contributors, still unresolved.
5. **The Versioning Trust Gap** (Aequitas #209) — When a tool ships breaking changes without a version bump, how can anyone trust its output? Fairness audits aren't just code — they're evidence.
6. **The Corporate Fairness Question** (Infosys RAI) — When a major IT company builds the fairness tooling, whose definition of "fair" becomes the default? Is enterprise fairness a public good or a proprietary standard?

---

## 💡 Listening Guide

| Episode arc | Project | Issue | Core tension |
|---|---|---|---|
| *"What Does Zero Mean?"* | AIF360 | #528 | Documentation vs. mathematical reality |
| *"The 360 That Isn't"* | AIF360 | #558 | Single scalar vs. intersectional justice |
| *"Framework vs. Instrument"* | FairLearn | #756 | General-purpose API vs. specialized tool |
| *"Even the Auditors Need Auditing"* | Aequitas | #201 | Self-referential documentation gaps |
| *"Who Version Who?"* | Aequitas | #209 | Trust, breaking changes, and evidence integrity |
| *"The Corporate Toolkit"* | Infosys RAI | — | Whose fairness standard ships by default? |

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*