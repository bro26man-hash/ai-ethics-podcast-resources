# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of **active open-source projects** related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects (Episode Research)

### 1. [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360) — *Comprehensive Fairness Metric Library & Mitigation*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |
| **Maintainer** | IBM Research (Trusted-AI org) |

**What it does:** A comprehensive set of fairness metrics for datasets and machine learning models, explanations for these metrics, and algorithms to mitigate bias. Covers 10+ fairness metrics (demographic parity, equalized odds, predictive parity, average odds, etc.) and 7+ mitigation algorithms (pre-processing, in-processing, post-processing). The most widely cited fairness toolkit in production and government contexts.

**Why it matters for the podcast:** AIF360 is the *de facto* standard for fairness measurement. When courts, regulators, and journalists cite fairness metrics, they often point to AIF360's definitions. But as Issue #528 reveals — the docstring for `average_odds_difference` says "a value of 0 indicates equality of odds" when that's mathematically wrong. If the most-cited toolkit ships a documentation error about what "zero" means, who owns the definition of fairness?

**🔴 Live Debate:** See [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The `average_odds_difference` metric is documented as indicating "equality of odds," but AndreFCruz proved there are configurations where the metric equals 0 yet equalized odds do NOT hold. After 17 months with no maintainer response, volunteer Hanabi9248 offered a concrete fix in September 2026 — but the issue remains unassigned and unmerged. See `DEBATES.md` for the full summary.

**🔴 Second Debate:** See [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Should the Empirical Differential Fairness metric return a single scalar, or should it break down discrimination by specific group combinations for intersectional analysis? jetverbeek proposed the enhancement; Hanabi9248 volunteered to implement it. Still unassigned.

```bash
pip install aif360
from aif360.metrics import ClassificationMetric
# Try computing average_odds_difference vs. equalized_odds_difference on the same dataset
# See if you can find a case where AOD=0 but EOD≠0
```

**Podcast angle:** When the most-cited fairness toolkit in the world ships a metric docstring that misstates what "zero" means, and a volunteer tries to fix it but no maintainer merges the correction for 17 months — what does that say about who controls the definition of fairness?

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

**🔴 Live Debate:** See [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — MetricFrame API design: should the tool support metrics that don't require `y_true` and `y_pred`? This is a fundamental question about **who the tool serves** — classification/regression practitioners only, or also researchers working on contextual bandits, cost-sensitive learning, and streaming data? The 74-comment thread (open since April 2021) reveals a deep divide between simplicity and generality. See `DEBATES.md` for the full summary.

```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame
# Try the current API vs. the proposed flexible API in Issue #756
```

**Podcast angle:** Should a fairness tool be limited to the metrics its founders imagined, or should it evolve to serve unexpected use cases — even if that makes the API more complex? When does flexibility become confusion?

---

### 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — *Comprehensive Responsible AI Platform*

| | |
|---|---|
| **Stars** | 310 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Infosys (InfosysResponsibleAI) |

**What it does:** A comprehensive Responsible AI platform with modules for safety, security, privacy, explainability, **fairness & bias detection**, and hallucination detection for both LLMs and traditional ML models. The fairness module implements statistical parity difference, disparate impact ratio, four-fifths rule, and Cohen's D, plus mitigation via equalized odds and re-weighing. Recently added LLM benchmarking across fairness, privacy, truthfulness, and ethics.

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

### 4. [Themis-ML](https://github.com/cosmicbboy/themis-ml) — *Fairness-Aware ML Algorithms (Academic)*

| | |
|---|---|
| **Stars** | 126 ⭐ |
| **Language** | Jupyter Notebook |
| **License** | MIT |
| **Last Updated** | February 2026 |
| **Maintainer** | cosmicbboy (independent researcher) |

**What it does:** A Python library built on pandas and sklearn that implements fairness-aware machine learning algorithms. Defines discrimination as preference (bias) for or against social groups resulting in unfair treatment. Implements discrimination discovery methods (mean difference, normalized mean difference) and mitigation techniques (relabelling/massaging, additive counterfactually fair estimator, reject option classification).

**Why it matters for the podcast:** Themis-ML is the **academic counterpoint** to the big-toolkit approach. It's smaller, lighter, and built on a clear theoretical framework — the author's own paper on discrimination discovery. Its README explicitly states: *"A 'fair' algorithm depends on how we define fairness."* This is the project that least hides its normative choices behind automation. It's also the most honest about what's missing — many mitigation techniques are unchecked (`[ ]`), signaling that the library is a research prototype, not a production tool.

**Why it's different:** Unlike AIF360 (which tries to be comprehensive) or FairLearn (which tries to be practical), Themis-ML tries to be *principled*. It doesn't ship 20 metrics; it ships 2, and it tells you what they mean.

```bash
pip install themis-ml
from themis_ml import MeanDifference, Relabeling
# Start with the simplest possible fairness measurement
```

**Podcast angle:** Is it better to ship two well-understood metrics with clear definitions, or twenty metrics with ambiguous docstrings? Can a small, honest research tool teach us more about fairness than a large, comprehensive toolkit?

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

| Project | Stars | Focus | Best For | |
|---|---|---|---|---|
| **AIF360** | 2,866 ⭐ | Comprehensive metric library + mitigation | Understanding how fairness is defined in production/government contexts; **live debate in Issue #528** |
| **FairLearn** | 2,286 ⭐ | Fairness assessment + mitigation | Practitioners in Microsoft/MLOps ecosystems; **live debate in Issue #756** |
| **Infosys RAI Toolkit** | 310 ⭐ | Enterprise Responsible AI platform | Deploying fairness as a service; platform-vs-developer accessibility tension |
| **Themis-ML** | 126 ⭐ | Fairness-aware algorithms (academic) | Understanding the theory behind fairness; small, principled, honest about gaps |
| **Aequitas** | 773 ⭐ | Bias auditing + evidence-grade reporting | Auditors who need to produce documentation, not just metrics |
| **AIBF_API** | 195 ⭐ | Real-time bias detection in hiring ATS | The hiring-pipeline use case; post-hoc flagging vs. pre-decision interception |
| **Fair-Code** | 47 ⭐ | 7 full bias audits + dataset profiler | Domain-specific audits (COMPAS, healthcare, tenant screening) |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [Aequitas documentation](https://dssg.github.io/aequitas/) — DSG's audit toolkit docs
- [Themis-ML documentation](http://themis-ml.readthedocs.io/en/latest/) — Academic fairness library docs
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — average_odds_difference documentation bug (17 months unresolved)
- [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Extend EDF metric for intersectional analysis
- [AIF360 Issue #548](https://github.com/Trusted-AI/AIF360/issues/548) — Official website is down
- [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — MetricFrame API design debate (74 comments, open since 2021)
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
3. **The Missing-Value Contract** (FairLearn #1725) — Should fairness tools be strict or silent when sensitive data is missing?
4. **The MetricFrame API Design Debate** (FairLearn #756) — Should a fairness tool support metrics beyond classification/regression, even if it makes the API more complex? Who does the tool serve — practitioners or researchers? (See `DEBATES.md` for full summary)
5. **The Audit-Grade vs. Research-Grade Divide** (Aequitas #201, #86) — Should fairness tools be built for producing court-ready evidence, or for exploratory research?
6. **The Vendor Independence Question** (Oracle Guardian AI) — Can the same company that sells you the compute also audit the fairness of what runs on it?
7. **The Small-Honest-vs-Large-Comprehensive Question** (Themis-ML) — Is it better to ship two well-understood metrics with clear definitions, or twenty metrics with ambiguous docstrings?

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*