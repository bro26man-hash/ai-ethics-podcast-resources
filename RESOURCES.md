# 📚 Curated Open-Source Fairness & Bias-Auditing Projects

This page is a living catalog of the most active, well-documented, and philosophically interesting open-source projects working on algorithmic fairness and bias auditing. We include projects not just for their technical capabilities, but for the **debates they embody** — the unwinnable trade-offs, the contested definitions, and the questions about whom these tools actually serve.

---

## 🏆 Tier 1 — Major Toolkits (Production-Grade, Widely Cited)

### 1. [AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Owner**: Trusted-AI (IBM Research)
- **Stars**: ⭐ 2,866 | **Forks**: 914 | **License**: Apache-2.0
- **Language**: Python (also R)
- **Last active**: September 2026

**What it does**: A comprehensive toolkit with 70+ fairness metrics and 15+ bias-mitigation algorithms spanning preprocessing, in-processing, and post-processing. Covers group fairness metrics (demographic parity, equalized odds, etc.) and sample distortion metrics (Individual Fairness, Similarity-based Fairness).

**Why it matters for our podcast**: AIF360 is one of the most widely deployed fairness toolkits in industry and academia. Its documentation shapes how practitioners think about fairness — which means when its metric definitions are contested (see [Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)), the stakes are enormous. **Debate spotlight**: Is `average_odds_difference` actually an equalized-odds relaxation? The repo's own docs say so, but a community contributor argues the math doesn't hold up — and the error has propagated to IBM's official documentation.

**Key algorithms**: Optimized Preprocessing, Disparate Impact Remover, Equalized Odds Postprocessing, Reweighing, Adversarial Debiasing, Exponentiated Gradient Reduction, and more.

---

### 2. [Fairlearn](https://github.com/fairlearn/fairlearn)
- **Owner**: fairlearn (Microsoft)
- **Stars**: ⭐ 2,286 | **Forks**: 516 | **License**: MIT
- **Language**: Python
- **Last active**: September 2026

**What it does**: A Python package for assessing and improving fairness in ML models. Provides grouping metrics (through `MetricFrame`), fairness constraints, and mitigation algorithms (Reductions, Postprocessing, Preprocessing).

**Why it matters**: Fairlearn explicitly frames fairness as a **sociotechnical challenge** — their README states: *"Fairness is fundamentally a sociotechnical challenge. Many aspects of fairness, such as justice and due process, are not captured by quantitative fairness metrics."* This philosophical self-awareness makes them a rich source of debate.

**Notable issues**:
- [#1725](https://github.com/fairlearn/fairlearn/issues/1725) — MetricFrame and `plot_roc_curve_by_group` disagree on how to handle missing sensitive feature values. This isn't just a bug — it reveals a deeper tension: **should fairness tools impute missing group labels, or refuse to evaluate when the data is incomplete?** The answer determines whether your model gets a clean bill of health or a "we can't tell" warning.
- [#756](https://github.com/fairlearn/fairlearn/issues/756) — Should `MetricFrame` support metrics that don't require `y_true` and `y_pred`? The 74-comment thread is a masterclass in the novices-vs-experts tension in fairness tooling.

---

### 3. [Aequitas](https://github.com/dssg/aequitas)
- **Owner**: Data and Society Project (University of Chicago)
- **Stars**: ⭐ 772 | **License**: MIT
- **Language**: Python
- **Last active**: Actively maintained

**What it does**: A bias audit toolkit designed for non-technical users. Generates a fairness audit report using confusion-matrix-based metrics, with an interactive web API for visual comparison across demographic groups.

**Why it matters**: Aequitas was built specifically for **civic tech and government use cases** — hiring, lending, criminal justice. Its design philosophy prioritizes accessibility over algorithmic completeness, which creates its own tensions: when you simplify fairness metrics for public consumption, what gets lost in translation?

---

## 🔬 Tier 2 — Specialized & Emerging Tools

### 4. [LiFT](https://github.com/linkedin/LiFT)
- **Owner**: LinkedIn
- **Stars**: ⭐ 173 | **License**: MIT
- **Language**: Scala/Spark
- **Last active**: Actively maintained

**What it does**: LinkedIn's toolkit for measuring fairness at web scale using permutation testing. Designed for massive datasets where traditional sampling-based fairness metrics become computationally intractable.

**Why it matters**: LiFT raises the question: **can fairness be measured without sampling bias?** When your dataset has millions of records, Monte Carlo methods introduce their own uncertainties. LiFT's permutation approach avoids this — but at the cost of assuming exchangeability under the null hypothesis, which is itself a controversial statistical assumption.

---

### 5. [Responsibly](https://github.com/ResponsiblyAI/responsibly)
- **Owner**: ResponsiblyAI
- **Stars**: ⭐ 100 | **License**: MIT
- **Language**: Python
- **Last active**: Actively maintained

**What it does**: A Python-first auditing toolkit aligned with the textbook *Fairness and Machine Learning* (Barocas, Hardt & Narayanan). Includes word-embedded bias metrics, credit scoring fairness audits, and COMPAS recidivism prediction analysis.

**Why it matters**: Responsibly bridges the gap between academic fairness research and practitioner tools. Its alignment with Barocas, Hardt & Narayanan's framework means it implicitly endorses their **"hydrostatic" theory of fairness** — the idea that fairness constraints should be understood as pressure equalization in a social hydrograph. Not everyone agrees with this framing.

---

## 🔧 Tier 3 — Niche & Community Projects

### 6. [Fair-Code](https://github.com/yakew7/Fair-Code)
- **Stars**: ⭐ 47 | **License**: Open
- **Language**: HTML
- **Last active**: September 2026

**What it does**: Seven open-source audits of algorithmic bias across criminal justice, hiring, lending, healthcare, welfare, and tenant screening. Provides measurable fairness goals for each domain.

**Why it matters**: Fair-Code is domain-specific audit code, not a general-purpose toolkit. This reflects a growing movement: **domain-specific fairness audits** that reject the one-size-fits-all metric approach. What counts as "fair" in hiring looks nothing like what counts as "fair" in healthcare triage.

---

### 7. [FairScan](https://github.com/Cypher-redeye/FairScan)
- **Stars**: ⭐ 1 | **License**: Open
- **Language**: JavaScript
- **Last active**: June 2026

**What it does**: AI-powered bias auditing tool that detects demographic disparities in datasets and explains findings in plain English using Gemini AI.

**Why it matters**: FairScan represents the emerging category of **LLM-augmented fairness auditing**. Using generative AI to explain bias findings to non-technical stakeholders is powerful — but it introduces a new layer of opacity. If the explainer itself is a black box, have we really solved the explainability problem, or just moved it?

---

## 🗺️ The Fairness Tooling Landscape

| Dimension | AIF360 | Fairlearn | Aequitas | LiFT | Responsibly |
|-----------|--------|-----------|----------|------|-------------|
| **Primary audience** | Practitioners & researchers | Practitioners | Non-technical / civic | Engineers at scale | Auditors & researchers |
| **Fairness paradigm** | Group + individual | Group | Group (confusion matrix) | Group | Group (textbook) |
| **Mitigation included** | ✅ 15+ algorithms | ✅ Reductions, post-processing | ❌ Assessment only | ❌ Measurement only | ❌ Assessment only |
| **Intersectionality** | Partial (rich subgroups) | Via custom groupings | Limited | Limited | Via custom groupings |
| **Scalability** | Medium (Python) | Medium (Python) | Medium (Python) | **High (Spark)** | Medium (Python) |
| **Open license** | Apache-2.0 | MIT | MIT | MIT | MIT |

---

## 📖 Further Reading

- **[Fairness and Machine Learning](https://www.fairnessml.com/)** — Barocas, Hardt & Narayanan's free textbook (the "bible" of the field)
- **[Monetary Fairness](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3637368)** — Kleinberg, Mullainathan & Raghavan on immeasurable fairness
- **[Inherent Trade-Offs in the Fair Determination of Risk Scores](https://arxiv.org/abs/1609.05807)** — Kleinberg et al. on the impossibility theorem
- **[A Guide to Fairness Metrics](https://join.ai/guides/fairness-metrics/)** — Practical overview of metric definitions and when to use each

---

*This page is community-maintained. To suggest additions or corrections, [open an issue](https://github.com/bro26man-hash/ai-ethics-podcast-resources/issues/new).*