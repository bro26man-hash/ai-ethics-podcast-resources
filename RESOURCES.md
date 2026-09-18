# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of **active open-source projects** related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects (Episode Research)

### 1. [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn) — *Fairness Assessment & Mitigation*

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

### 2. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — *Comprehensive Responsible AI Platform*

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

### 3. [Fair-Code — Yakew7](https://github.com/yakew7/Fair-Code) — *7 Open-Source Algorithmic Bias Audits* 🔴

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Language** | Python / HTML |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainer** | Yash Kewlani (@yakew7) |

**What it does:** Seven full fairness audits across criminal justice (COMPAS), hiring, lending (German Credit), healthcare (insurance denial, readmission), welfare eligibility, and tenant screening. Each ships as runnable `unfair.py` / `fair.py` scripts plus Jupyter notebooks. Includes a dataset profiler, benchmark harness, and **61 plain-language explainers** of fairness concepts.

**Why it matters for the podcast:** Fair-Code is the most *poetically fitting* project for our theme. It's a solo-built tool that exposes bias in systems that affect freedom, jobs, and healthcare — and it's currently the site of a **live, unresolved documentation controversy**. The maintainer discovered that his own project's documentation cited **fabricated fairness numbers** — a tool designed to catch numerical inaccuracies in fairness claims had itself produced inaccurate fairness claims in its docs. This is the kind of meta-irony that makes for compelling podcast material.

**🔴 Live Debate:** See [Fair-Code Issue #521](https://github.com/yakew7/Fair-Code/issues/521) — *"model-drift.md cites a fabricated single-run gap/p-value for unfair.py (6.39%/p=0.348 instead of the real 7.16%/p=0.2564)"*. The maintainer found the wrong number, a contributor reproduced it and got a *different* wrong number on a different platform, and the thread reveals that **even the numbers in a fairness tool's documentation can't agree across machines**. See `DEBATES.md` for the full summary with quoted arguments.

**Also see:** [Fair-Code Issues #660–#672](https://github.com/yakew7/Fair-Code/issues?q=is%3Aopen+label%3Adocumentation) — 13 documentation-error issues filed in a single day, including fabricated COMPAS TPR/FPR tables, wrong dataset column names, and incorrect reproducibility claims.

```bash
pip install -e ".[benchmark]"
faircode benchmark  # runs all 7 audits with 5 mitigation strategies × 3 model families × 6 fairness metrics
```

**Podcast angle:** When a fairness auditing tool's own documentation contains numbers that don't reproduce, who does the tool serve — readers who need accurate information, or maintainers who want their narrative to look consistent?

---

### 4. [Oracle Guardian AI](https://github.com/oracle/guardian-ai) — *Fairness & Privacy Assessment Library*

| | |
|---|---|
| **Stars** | 53 ⭐ |
| **Language** | Python |
| **License** | Open source |
| **Last Updated** | September 2026 |
| **Maintainer** | Oracle |

**What it does:** A library for assessing fairness/bias and privacy of machine learning models and datasets. Designed for enterprise governance contexts, providing auditable assessments that can feed into compliance workflows.

**Why it matters for the podcast:** Oracle's entry into the fairness toolkit space signals that **bias auditing is becoming a enterprise governance requirement**, not just a research exercise. Unlike AIF360 (IBM Research) or FairLearn (Microsoft), Oracle's toolkit is from a major cloud provider that also sells the infrastructure the models run on. This raises an uncomfortable question: **can the same company that sells you the compute also audit the fairness of what runs on it?** Is there an inherent conflict of interest in vendor-provided fairness tools?

```bash
pip install guardian-ai
# Oracle's fairness and privacy assessment toolkit
```

**Podcast angle:** When the cloud provider that sells you the GPU also provides the fairness audit, is the audit independent? Should we trust vendor-provided bias assessments the way we trust vendor-provided security patches?

---

## 🧩 Bonus: Smaller but Significant Projects

### [AIBF_API — jbarach2012](https://github.com/jbarach2012/AIBF_API) — *Explainable Bias-Detection Firewall for Hiring ATS*

| | |
|---|---|
| **Stars** | 195 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |

An open-source, explainable bias-detection firewall for Applicant Tracking Systems. AIBF intercepts an ATS scoring decision, measures how much of it was driven by protected-attribute proxies rather than merit, explains why in plain language, and flags biased decisions for human review.

### [IBM AIF360 — Trusted-AI](https://github.com/Trusted-AI/AIF360) — *The Granddaddy*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Language** | Python & R |
| **License** | Apache-2.0 |

The most comprehensive fairness toolkit — 70+ metrics, 11 bias mitigation algorithms. Current live debates: [#528 — Average-Odds Documentation Bug](https://github.com/Trusted-AI/AIF360/issues/528) (metric docstring misstates what "zero" means) and [#558 — Intersectional Analysis Enhancement](https://github.com/Trusted-AI/AIF360/issues/558) (should the tool return a scalar or a breakdown?).

### [Fairness.jl — Ashrya Agr](https://github.com/ashryaagr/Fairness.jl) — *The Performance Angle*

| | |
|---|---|
| **Stars** | 33 ⭐ |
| **Language** | Julia |

A Julia toolkit providing fairness metrics and bias mitigation algorithms. Julia's performance advantages make it suitable for large-scale fairness audits that would be too slow in Python. Raises the question: *does the choice of programming language expand or constrain who can participate in fairness auditing?*

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Stars | Focus | Best For | Live Debate |
|---|---|---|---|---|
| **AIF360** | 2,866 ⭐ | Comprehensive metric library + mitigation | Understanding how fairness is defined in production/government contexts | [#528 — Docstring bug](https://github.com/Trusted-AI/AIF360/issues/528) |
| **FairLearn** | 2,286 ⭐ | Fairness assessment + mitigation | Practitioners in Microsoft/MLOps ecosystems | [#756 — MetricFrame API](https://github.com/fairlearn/fairlearn/issues/756) |
| **Infosys RAI** | 310 ⭐ | Enterprise Responsible AI platform | Deploying fairness as a service | [#71 — Language access](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit/issues/71) |
| **Fair-Code** | 47 ⭐ | 7 full bias audits + dataset profiler | Domain-specific audits + documentation accountability | [#521 — Fabricated numbers](https://github.com/yakew7/Fair-Code/issues/521) |
| **Oracle Guardian** | 53 ⭐ | Fairness & privacy assessment | Enterprise governance; vendor conflict-of-interest | Open issues pending |
| **AIBF_API** | 195 ⭐ | Real-time bias detection in hiring ATS | The hiring-pipeline use case | Post-hoc flagging vs. pre-decision interception |
| **Fairness.jl** | 33 ⭐ | Julia-first fairness metrics | Performance-critical audits; language accessibility question | Open issues pending |

---

## 📖 Essential Explainers & External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [Aequitas documentation](https://dssg.github.io/aequitas/) — DSG's audit toolkit docs
- [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — MetricFrame API design debate (74 comments, open since 2021)
- [Fair-Code Issue #521](https://github.com/yakew7/Fair-Code/issues/521) — Fabricated fairness numbers (the 6.39% vs 7.16% gap)
- [Fair-Code Issues #660–#672](https://github.com/yakew7/Fair-Code/issues?q=is%3Aopen+label%3Adocumentation) — 13 documentation errors filed in one day
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainasan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

## 🎙️ How These Projects Connect to the Podcast

Each episode zooms in on a **specific ongoing disagreement** from an open issue thread:

1. **The MetricFrame API Design Debate** (FairLearn #756) — Should a fairness tool be a general-purpose framework or a specialized instrument? Who decides which use cases it serves?
2. **The Average-Odds Documentation Bug** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition?
3. **The Intersectional Analysis Gap** (AIF360 #558) — Should fairness tools return a single scalar, or should they break down discrimination by specific group combinations?
4. **The Missing-Value Contract** (FairLearn #1725) — Should fairness tools be strict or silent when sensitive data is missing?
5. **The Fabricated Numbers Controversy** (Fair-Code #521) — When a fairness auditing tool's own documentation cites numbers that don't reproduce on different hardware, who does the tool serve — readers or maintainers? Is the 6.39% vs 7.16% gap a bug, a hardware sensitivity, or a narrative choice?
6. **The Documentation Decay Pattern** (Fair-Code #660–#672) — When 13 documentation errors are filed in a single day, is the project's documentation a public service or a liability?
7. **The Language Access Gap** (Infosys #71) — When a "good first issue" masks structural exclusion (the toolkit can't even handle Hinglish text), who gets designed in and who gets designed out?
8. **The Vendor Independence Question** (Oracle Guardian AI) — Can the same company that sells you the compute also audit the fairness of what runs on it?

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*