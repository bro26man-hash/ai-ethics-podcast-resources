# 📚 Open-Source Resources: Algorithmic Fairness & Bias Auditing

A curated collection of **active, well-maintained open-source projects** working on the harder problems of AI fairness — researched for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Key Projects (Episode Research)

### 1. AI Fairness 360 (AIF360) — Trusted-AI / IBM Research

- **Repo:** https://github.com/Trusted-AI/AIF360
- **Stars:** ⭐ 2,866
- **Language:** Python (also R)
- **License:** Apache-2.0
- **Last active:** September 2026

**What it does:** A comprehensive toolkit with metrics for detecting bias in datasets and models, explanations for those metrics, and algorithms to mitigate bias across the AI lifecycle. Covers 15+ mitigation algorithms from pre-processing (reweighing, optimized preprocessing) through in-processing (adversarial debiasing) to post-processing (equalized odds, calibrated equalized odds).

**Why it matters for the podcast:** AIF360 is one of the most widely cited fairness toolkits in the world, built at IBM Research. It's also the site of two active debates that define the core tensions in this space:
- **[Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)** — Does the `average_odds_difference` metric's documentation misleadingly claim "a value of 0 indicates equality of odds" when it doesn't? (See DEBATES.md)
- **[Issue #558](https://github.com/Trusted-AI/AIF360/issues/558)** — Should the Empirical Differential Fairness metric return just a single scalar, or should it break down which specific group combinations drive discrimination? (See DEBATES.md)

```bash
pip install aif360
from aif360.metrics import ClassificationMetric
```

---

### 2. Fairlearn — Microsoft

- **Repo:** https://github.com/fairlearn/fairlearn
- **Stars:** ⭐ 2,286
- **Language:** Python
- **License:** MIT
- **Last active:** September 2026

**What it does:** A Python package that empowers developers to assess and improve fairness of ML models. Defines fairness in terms of *harms* — allocation harms (withholding opportunities) and quality-of-service harms (uneven system performance). Provides both assessment metrics and mitigation algorithms designed for scikit-learn workflows.

**Why it matters for the podcast:** Fairlearn explicitly frames fairness as a **sociotechnical challenge**, not just a mathematical one. Its documentation states plainly that "many aspects of fairness, such as justice and due process, are not captured by quantitative fairness metrics." It's also the site of an active debate about whether fairness tools should be strict (raise errors on missing data) or permissive (silently skip rows) — see [FairLearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725).

```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame
```

---

### 3. Infosys Responsible AI Toolkit — Infosys

- **Repo:** https://github.com/Infosys/Infosys-Responsible-AI-Toolkit
- **Stars:** ⭐ 310
- **Language:** Python
- **License:** MIT
- **Last active:** September 2026

**What it does:** A broader enterprise platform for responsible AI, covering safety, security, privacy, explainability, fairness/bias detection, and hallucination detection for both LLMs and traditional ML models. Includes a Fairness & Bias module with statistical parity difference, disparate impact ratio, four-fifths rule, and Cohen's D for detection; equalized odds and re-weighing for mitigation.

**Why it matters for the podcast:** This toolkit represents the **enterprise/commercial approach** to responsible AI — full-suite, API-driven, designed for organizational compliance. It raises a distinct question from the academic toolkits: **when fairness is bundled with safety, privacy, and security as part of a commercial product, does the fairness component get shortchanged?** The metric coverage is narrower than AIF360's 20+ metrics.

```bash
pip install responsible-ai-fairness
```

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Stars | Focus | Best For |
|---|---|---|---|
| **AIF360** | 2,866 ⭐ | Comprehensive metric library + mitigation | Understanding how fairness is defined in production/government contexts; **two live debates in #528 & #558** |
| **Fairlearn** | 2,286 ⭐ | Fairness assessment + mitigation | Practitioners in Microsoft/MLOps ecosystems; **missing-data debate in #1725** |
| **Infosys RAI** | 310 ⭐ | Enterprise responsible AI suite | Organizations needing fairness bundled with safety, privacy, and compliance |

---

## 📖 Essential External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [FairLearn documentation](https://fairlearn.readthedocs.io/) — Microsoft's fairness toolkit docs
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainasan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

## 🎙️ How These Projects Connect to the Podcast

Each episode zooms in on a **specific ongoing disagreement** from an open issue thread:

1. **The Documentation Accuracy Debate** (AIF360 #528) — When the most-cited fairness toolkit ships a metric docstring that misstates what "zero" means, who owns the definition?
2. **The Intersectional Analysis Gap** (AIF360 #558) — Should fairness tools serve intersectional communities, or is a single scalar sufficient for regulators?
3. **The Missing-Value Contract** (FairLearn #1725) — Should fairness tools raise errors on missing sensitive data, or handle it gracefully?
4. **The Enterprise-vs-Academic Tension** (Infosys RAI) — When fairness is one module in a commercial suite, does it get the same depth as standalone academic toolkits?

*Contributors: Add your favourite fairness project — follow the table format above and include stars, license, and a one-line pitch.*