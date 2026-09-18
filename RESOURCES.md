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

**What it does:** An extensible open-source toolkit containing 15+ fairness metrics for datasets and models, explanations for those metrics, and algorithms to mitigate bias throughout the AI lifecycle. Covers group fairness (demographic parity, equalized odds, predictive parity, rich subgroup fairness), sample distortion metrics, and differential fairness. Includes mitigation algorithms spanning preprocessing (Optimized Preprocessing, Disparate Impact Remover), in-processing (Adversarial Debiasing, Fairness Constraints), and postprocessing (Equalized Odds Postprocessing, Calibrated Equalized Odds).

**Why it matters for the podcast:** AIF360 is the most widely cited fairness toolkit in production and government contexts. It's the toolkit courts reference, regulators cite, and engineering teams import. But it's also the toolkit with the most active documentation debates — including **Issue #528**, where a volunteer proved that the `average_odds_difference` metric's docstring wrongly claims "a value of 0 indicates equality of odds." The error has been open since April 2024 with no maintainer merge. See `DEBATES.md` for the full breakdown.

```bash
pip install aif360
from aif360.metrics import ClassificationMetric
# Try the controversial metric — and check whether the docstring matches the math
```

**🔴 Live Debate:** [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — *The Average-Odds Documentation Bug*

---

### 2. [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn) — *Fairness Assessment & Mitigation for MLOps*

| | |
|---|---|
| **Stars** | 2,286 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Microsoft (fairlearn org) |

**What it does:** A Python package to assess and improve fairness of ML models, designed for seamless integration with scikit-learn workflows. Supports key metrics (demographic parity, equalized odds, predictive parity, accuracy parity) and mitigation algorithms (threshold optimization, re-weighting, adversarial debiasing). Features `MetricFrame` for disaggregated metric computation and `plot_roc_curve_by_group` for group-aware visualization.

**Why it matters for the podcast:** FairLearn is Microsoft's answer to AIF360 — same core metrics, different design philosophy. Where AIF360 is an IBM Research project with academic roots, FairLearn is built for practical ML pipelines. The two toolkits sometimes define and compute the same metrics slightly differently, raising the question: **if two mainstream fairness tools disagree on a metric's definition, which one is "right"?** FairLearn also has its own live debate in Issue #1725, where `MetricFrame` and `plot_roc_curve_by_group` disagree on what to do when sensitive feature values are missing — forcing a confrontation about whether fairness tools should be strict or silent.

```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame, plot_roc_curve_by_group
# Try both APIs with missing sensitive features — and watch them disagree
```

**🔴 Live Debate:** [FairLearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725) — *The Missing-Value Contract*

---

### 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — *Enterprise Responsible AI Platform*

| | |
|---|---|
| **Stars** | 310 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Infosys (InfosysResponsibleAI org) |

**What it does:** A comprehensive Responsible AI platform with modules for safety, security, privacy, explainability, **fairness & bias detection**, and hallucination detection for both LLMs and traditional ML models. The fairness module implements statistical parity difference, disparate impact ratio, four-fifths rule, and Cohen's D, plus mitigation via equalized odds and re-weighing. Recently added LLM benchmarking across fairness, privacy, truthfulness, and ethics — making it one of the few toolkits that audits fairness in generative AI, not just classical ML.

**Why it matters for the podcast:** This toolkit represents the **enterpriseification** of fairness — it's not just a research library but a deployable platform with Azure OpenAI integration, a micro-frontend UI, and Kubernetes deployment. The fairness metrics are standard (same as AIF360/FairLearn), but the packaging asks: **should fairness tools be developer-first (command-line, scriptable) or platform-first (UI, deployment pipelines)?** Each choice implicitly decides who can use the tool — and who's excluded by the platform requirements.

**Active issues:**
- [#90](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit/issues/90) — Python 3.9.13 base image is stale, breaks pip installs (open since Sept 2026)
- [#71](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit/issues/71) — Support Romanized Indian Languages in Guardrails (open since March 2026, 3 comments)
- [#61](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit/issues/61) — Unified Containerization, CI/CD, and K8s Deployment (open since Oct 2025, 2 comments)

```bash
pip install responsible-ai-fairness
# Fairness & Bias module for LLM and traditional ML models
```

**Podcast angle:** When a fairness toolkit is bundled as an enterprise platform with Azure dependencies and Kubernetes requirements, who gets to use it? Does the platform complexity itself become a form of exclusion — fairness tools that only large corporations can afford to deploy?

---

## 📖 Bonus: Related Projects

### [AIBF_API — jbarach2012](https://github.com/jbarach2012/AIBF_API) — *Explainable Bias-Detection Firewall for Hiring ATS*

| | |
|---|---|
| **Stars** | 195 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |

An open-source, explainable bias-detection firewall for Applicant Tracking Systems. AIBF intercepts an ATS scoring decision, measures how much of it was driven by protected-attribute proxies rather than merit, explains why in plain language, and flags biased decisions for human review.

### [Fair-Code — yakew7](https://github.com/yakew7/Fair-Code) — *7 Open-Source Algorithmic Bias Audits*

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Language** | Python / HTML |
| **License** | MIT |

Seven full fairness audits across criminal justice (COMPAS), hiring, lending, healthcare, welfare eligibility, and tenant screening. Each ships as runnable scripts plus Jupyter notebooks. Includes a dataset profiler, benchmark harness, and 61 plain-language explainers.

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Stars | Focus | Best For | Live Debate |
|---|---|---|---|---|
| **AIF360** | 2,866 ⭐ | Comprehensive metric library + mitigation | Understanding how fairness is defined in production/government contexts | **#528** — Avg-Odds docstring bug (17+ months open) |
| **FairLearn** | 2,286 ⭐ | Fairness assessment + mitigation | Practitioners in Microsoft/MLOps ecosystems | **#1725** — Missing-value contract (Sept 2026) |
| **Infosys RAI Toolkit** | 310 ⭐ | Enterprise Responsible AI platform | Deploying fairness as a service; platform-vs-developer accessibility | #90, #71 — Infrastructure & localization |
| **AIBF_API** | 195 ⭐ | Real-time bias detection in hiring ATS | The hiring-pipeline use case; post-hoc flagging vs. pre-decision interception | — |
| **Fair-Code** | 47 ⭐ | 7 full bias audits + dataset profiler | Domain-specific audits (COMPAS, healthcare, tenant screening) | #672 — SHAP denominator choice |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation (see also: Issue #528 about a docstring error)
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [AIF360 Interactive Experience](https://aif360.res.ibm.com/data) — Gentle intro to fairness concepts (website currently down per Issue #548)
- [AIF360 Slack](https://aif360.slack.com) — Community channel (invitation link in repo)
- [Bellamy et al. (2018): AIF360 Technical Paper](https://arxiv.org/abs/1810.01943) — The seminal paper describing the toolkit
- [Hardt et al. (2016): Equality of Opportunity in Supervised Learning](https://papers.nips.cc/paper/6374-equality-of-opportunity-in-supervised-learning) — Equalized odds foundation
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — Impossibility theorem
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate

---

## 🎙️ How These Projects Connect to the Podcast

Each episode zooms in on a **specific ongoing disagreement** from an open issue thread:

1. **The Average-Odds Documentation Bug** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition? A volunteer proved the error with a four-row counterexample. Seventeen months later, no maintainer has merged the fix.

2. **The Missing-Value Contract** (FairLearn #1725) — Should fairness tools be strict or silent when sensitive data is missing? `MetricFrame` raises a `ValueError`; `plot_roc_curve_by_group` silently drops rows. The maintainer has now confirmed: both should raise. But the fix is still pending.

3. **The Intersectional Analysis Gap** (AIF360 #558) — Should fairness tools return a single scalar, or should they break down discrimination by specific group combinations? A volunteer offered to implement the enhancement. The issue sits unassigned.

4. **The Generalist-vs-Specialist Tension** (FairLearn #756) — Should a fairness tool support metrics beyond classification, even if it makes the API more complex? Who does the tool serve — practitioners or researchers? (74 comments, open since 2021)

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*