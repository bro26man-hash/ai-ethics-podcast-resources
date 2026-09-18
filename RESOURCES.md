# 📚 Curated Open-Source Fairness & Bias-Auditing Projects

This page links the active open-source projects we track on the podcast. Each entry includes a short description, why it matters for the social-justice angle, and where to find live debates (issue threads, PR discussions).

---

## 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)

| | |
|---|---|
| **Owner** | Trusted-AI (IBM) |
| **Stars** | ⭐ 2,866 |
| **Language** | Python (also R) |
| **License** | Apache-2.0 |
| **Last active** | Updated September 2026 |

**What it is:** An extensible open-source toolkit containing a comprehensive set of fairness metrics for datasets and models, explanations for those metrics, and algorithms to mitigate bias throughout the AI lifecycle. Covers 15+ bias-mitigation algorithms (pre-processing, in-processing, post-processing) and metrics like Demographic Parity, Equalized Odds, Predictive Parity, Average Odds, and Rich Subgroup Fairness.

**Why it matters for social justice:** AIF360 was designed to translate algorithmic fairness research into real-world practice across finance, hiring, healthcare, and education. Its metrics are referenced in policy discussions around the EU AI Act and NIST AI RMF. When a metric is mislabeled or misunderstood in this toolkit, it can mislead practitioners who build high-stakes systems affecting marginalized communities.

**Where to find live debate:** See [Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — "`average_odds_difference` metric is wrongly represented as an equalized odds relaxation" — a mathematical dispute about whether the toolkit's own documentation falsely equates a metric with a fairness criterion it does not actually satisfy. Also see [Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — a request to extend the Empirical Differential Fairness metric for intersectional analysis.

---

## 2. [Aequitas](https://github.com/dssg/aequitas)

| | |
|---|---|
| **Owner** | David B. Lawrence III (dssg) |
| **Stars** | ⭐ 773 |
| **Language** | Python |
| **License** | MIT |
| **Last active** | Updated September 2026 |

**What it is:** A bias-auditing and fairness-ML toolkit designed to help data scientists detect, analyze, and mitigate bias in predictive models. It provides a systematic framework for generating fairness reports with group metrics, individual metrics, and intersectional analysis.

**Why it matters for social justice:** Aequitas emphasizes the audit trail — not just "is this model fair?" but "how do you prove it, and to whom?" Its intersectional analysis capabilities surface disparities that aggregate metrics can hide, which is crucial for communities that are multiply-marginalized.

**Open issues to watch:**
- [Issue #201](https://github.com/dssg/aequitas/issues/201) — "Add a readme or page on the existing metrics of fairness" — a documentation gap that makes it harder for non-experts to understand what each metric actually measures.
- [Issue #116](https://github.com/dssg/aequitas/issues/116) — "deal with multiclass problems" — fairness metrics are largely designed for binary classification; extending them to multiclass contexts (common in real-world scoring) is an open research challenge.

---

## 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)

| | |
|---|---|
| **Owner** | Infosys |
| **Stars** | ⭐ 310 |
| **Language** | Python |
| **License** | Apache-2.0 |
| **Last active** | Updated September 2026 |

**What it is:** A comprehensive responsible-AI toolkit covering fairness, explainability, and compliance. Provides metrics for group and individual fairness, bias detection, and mitigation algorithms, plus governance features for regulatory alignment.

**Why it matters for social justice:** As an enterprise-grade toolkit from a major IT services company, it represents the bridge between academic fairness research and corporate deployment. Its governance features raise questions about whether fairness can be "managed" at scale — and who sets the compliance bar.

---

## 4. [Fairlearn](https://github.com/fairlearn/fairlearn) ⭐ *NEW*

| | |
|---|---|
| **Owner** | Microsoft |
| **Stars** | ⭐ 2,286 |
| **Language** | Python |
| **License** | MIT |
| **Last active** | Updated September 2026 |

**What it is:** A Python package with two core pillars: (1) **Assessment metrics** for revealing which groups are harmed by a model, and (2) **Mitigation algorithms** for reducing unfairness across defined fairness constraints. Covers group fairness definitions including demographic parity, equalized odds, and predictive parity.

**Why it matters for the podcast:** Fairlearn explicitly frames fairness as a **sociotechnical** challenge. Its documentation states: *"Fairness is fundamentally a sociotechnical challenge. Many aspects of fairness, such as justice and due process, are not captured by quantitative fairness metrics. Furthermore, there are many quantitative fairness metrics which cannot all be satisfied simultaneously."* This philosophical self-awareness makes Fairlearn a perfect foil to AIF360's more engineering-oriented approach — and raises the question: **can a tool that admits its own limitations still be trusted to define fairness?**

**Potential tension with AIF360:** Fairlearn and AIF360 provide overlapping metrics but may define or compute them differently. If two mainstream tools disagree on what "average odds difference = 0" means (as raised in AIF360 Issue #528), practitioners and regulators have no single authoritative reference. This is a live question for the episode.

**Run it locally:**
```bash
pip install fairlearn
from fairlearn.metrics import MetricFrame
```

---

## 5. [Algorithm-Books](https://github.com/manjunath5496/Algorithm-Books) ⭐ *NEW*

| | |
|---|---|
| **Owner** | manjunath5496 |
| **Stars** | ⭐ 366 |
| **Language** | Not specified |
| **License** | Not specified |
| **Last active** | Updated September 2026 |

**What it is:** A curated reading list and commentary on algorithms, computation, and social justice. The repo's tagline is a clause from Zoe Quinn: *"Algorithms are not arbiters of objective truth and fairness simply because they're math."*

**Why it matters for the podcast:** This is the most philosophically direct project in our collection. While AIF360, Fairlearn, and Aequitas provide the tools, Algorithm-Books provides the **critique** — the reminder that mathematical formalism does not confer moral authority. It's a short but punchy resource that frames the entire conversation. Perfect for the episode's opening: before we talk about metrics, we need to talk about **who gets to speak**.

---

## 🔍 Quick Reference Matrix

| Project | Maintainer | Stars | Approach | Best For |
|---|---|---|---|---|
| **AIF360** | IBM / Trusted-AI | 2,866 | Comprehensive metric library + mitigation | Production/government contexts; understanding de facto standards |
| **Aequitas** | dssg | 773 | Audit-first with intersectional focus | Proving fairness to non-experts; audit trail requirements |
| **Infosys RAI** | Infosys | 310 | Enterprise governance + fairness | Corporate compliance; regulatory alignment |
| **Fairlearn** | Microsoft | 2,286 | Sociotechnical framing + assessment/mitigation | Philosophical grounding; contrasting with AIF360's approach |
| **Algorithm-Books** | Community | 366 | Critical reading list + provocations | Framing the ethical/political context before diving into metrics |

---

## 📖 Essential External Resources

- [AIF360 documentation](https://aif360.readthedocs.io/en/stable/) — IBM's official metric documentation
- [Fairlearn User Guide](https://fairlearn.org/main/user_guide/index.html) — Microsoft's fairness framework docs
- [Fairlearn on Fairness Definitions](https://fairlearn.org/main/user_guide/fairness_in_machine_learning.html) — Sociotechnical framing
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — Impossibility theorem
- [Kleinberg et al. (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — Causal fairness framework
- [AIF360 Technical Paper (Bellamy et al., 2018)](https://arxiv.org/abs/1810.01943) — Foundational toolkit paper

---

*Contributors: Add your favourite fairness project above — follow the table format and include stars, license, and a one-line pitch. See `DEBATES.md` for summaries of live controversies from issue threads.*