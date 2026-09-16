# 🔍 Curated Open-Source Fairness & Bias-Auditing Projects

A companion to the AI Ethics & Social Justice Podcast's investigation into how the
open-source fairness tooling community measures, debates, and attempts to repair
algorithmic harm. These three projects were selected for active maintenance,
substantial community adoption, and genuine internal disagreements about what
fairness means and who a fairness tool should serve.

---

## 1. Microsoft Fairlearn

| | |
|---|---|
| **Repository** | [fairlearn/fairlearn](https://github.com/fairlearn/fairlearn) |
| **Language** | Python |
| **Stars** | ⭐ 2,285 |
| **License** | MIT |
| **Updated** | September 2026 |
| **Maintainer** | Microsoft Research / Incident IO |

**What it does:** Fairlearn is a Python package that helps developers assess and
improve fairness in machine learning models. It provides `MetricFrame` for
measuring group fairness metrics across sensitive features, reduction algorithms
(ExponentiatedGradient, GridSearch) that trade off accuracy for fairness,
and post-processing methods like Equalized Odds.

**Why it matters:** Fairlearn explicitly frames fairness as a
*sociotechnical* challenge, not just a technical one. Its documentation
includes sections on stakeholder identification, power dynamics, and the
history of discriminatory classification. It's one of the most cited
open-source fairness tools in both industry and academia.

**Key tension we tracked:** See [DEBATES.md](DEBATES.md) for the debate over
whether Fairlearn should recognize non-human animal species as a "sensitive
feature" — a question that cuts to the heart of what a fairness framework
is *for*.

**Notable open issues:**
- [#1625](https://github.com/fairlearn/fairlearn/issues/1625) — Proposal to consider species as a sensitive feature
- [#1552](https://github.com/fairlearn/fairlearn/issues/1552) — Disagreement over pandas vs. Narwhals for data handling (26 comments)
- [#1522](https://github.com/fairlearn/fairlearn/issues/1522) — Maintenance: replacing pandas with narwhals (26 comments)

---

## 2. IBM AI Fairness 360 (AIF360)

| | |
|---|---|
| **Repository** | [Trusted-AI/AIF360](https://github.com/Trusted-AI/AIF360) |
| **Language** | Python & R |
| **Stars** | ⭐ 2,865 |
| **License** | Apache 2.0 |
| **Updated** | September 2026 |
| **Maintainer** | IBM Research / Linux Foundation AI &
Data (Trusted-AI org) |

**What it does:** AIF360 is the most comprehensive open-source fairness
toolkit available. It includes **70+ fairness metrics** covering group fairness,
individual fairness, and causal fairness; **15+ bias mitigation algorithms** spanning preprocessing, in-processing, and post-processing; and explainability modules. It supports binary and multiclass classification, regression, and ranking.

**Why it matters:** AIF360 is the standard reference implementation for many
fairness metrics used in both research and industry. Its `BinaryLabelDatasetMetric`
and `ClassificationMetric` classes are the basis for the numbers cited in
academic fairness papers. The toolkit is so mature it has its own R package
and interactive web demo at [aif360.res.ibm.com](https://aif360.res.ibm.com).

**Key tensions we tracked:**
- The "SpeciesistBiasMetric" proposal [#559](https://github.com/Trusted-AI/AIF360/issues/559)
  — should AIF360's bias-detection framework extend to non-human animal species?
- The FairBench collaboration proposal [#535](https://github.com/Trusted-AI/AIF360/issues/535)
  — who gets to define and curate the building blocks of fairness metrics?
- The intersectional bias mitigation request [#537](https://github.com/Trusted-AI/AIF360/issues/537)
  — how should we measure harm when multiple protected attributes overlap?

**Notable open issues:**
- [#559](https://github.com/Trusted-AI/AIF360/issues/559) — Proposal: add SpeciesistBiasMetric for evaluating species-based discrimination in NLP
- [#537](https://github.com/Trusted-AI/AIF360/issues/537) — Adding support for intersectional bias mitigation
- [#535](https://github.com/Trusted-AI/AIF360/issues/535) — Collaboration proposal: wrapping around the FairBench library's metric definitions

---

## 3. Responsibly

| | |
|---|---|
| **Repository** | [ResponsiblyAI/responsibly](https://github.com/ResponsiblyAI/responsibly) |
| **Language** | Python |
| **Stars** | ⭐ 101 |
| **License** | MIT |
| **Updated** | September 2026 |
| **Maintainer** | ResponsiblyAI (open-source community) |

**What it does:** Responsibly is a Python-first auditing toolkit for bias and
fairness in machine learning systems. It is explicitly aligned with the
_Fairness and Machine Learning_ framework by Barocas, Hardt, and Narayanan,
and includes word-embedded bias metrics for evaluating distributional
fairness in text-based models.

**Why it matters:** Responsibly bridges the gap between the academic Fairness
and Machine Learning literature and practical auditing workflows. Its
metric definitions are grounded in the Oxford handbook, and it provides a
clean API that mirrors the AIF360 and Fairlearn interfaces. It's growing
rapidly and has strong alignment with the FML fairness taxonomy.

**Why we included it:** While Fairlearn and AIF360 dominate the landscape,
Responsibly represents a newer generation of tools that are built specifically
to audit real-world ML pipelines rather than just provide metrics for research
papers. It's a useful contrast for thinking about how fairness tooling is
evolving from "assessment" to "audit."

**Notable context:**
- Aligned with Fairness and Machine Learning (FMB) taxonomy
- Python-first, lightweight, designed for integration into existing pipelines
- Active as of September 2026

---

## How to Contribute

Found another active fairness or bias-auditing project that belongs here?
Open an issue or send a PR with:

1. The repository URL
2. A brief description of what it does
3. Stars, license, last update date
4. Why it belongs in this list (especially if it centers affected communities)

We prioritize tools that are actively maintained, openly licensed, and that
acknowledge the communities they serve — not just the data scientists using them.
