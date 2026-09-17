# 📂 Open-Source Fairness & Bias-Auditing Projects

A curated collection of active, community-driven open-source projects working at the intersection of algorithmic fairness, bias auditing, and responsible AI. Each entry includes why it matters for the podcast and where to dive in.

---

## 🏆 Tier 1 — Major Toolkits (Production-Scale)

### 1. [Fairlearn](https://github.com/fairlearn/fairlearn) — `fairlearn/fairlearn`
- **Stars:** 2,249 | **Language:** Python | **License:** MIT
- **What it does:** Provides a Python package to assess and improve the fairness of machine learning models. Includes `MetricFrame` for disaggregated evaluation, fairness visualizations, and mitigation algorithms (reductions, post-processing, pre-processing).
- **Why it matters for the podcast:** Fairlearn explicitly frames fairness as a *sociotechnical* challenge — not just a math problem. Its maintainers actively debate how the tool should serve different users (novice vs. advanced, classification vs. reinforcement learning). The ongoing [MetricFrame API debate](https://github.com/fairlearn/fairlearn/issues/756) (74 comments, still open) is a perfect case study in the tensions we cover.
- **Get started:** `pip install fairlearn` | [Tutorials](https://fairlearn.org/)

### 2. [AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360) — `Trusted-AI/AIF360`
- **Stars:** 2,866 | **Language:** Python/R | **License:** Apache-2.0
- **What it does:** IBM's comprehensive toolkit containing 70+ fairness metrics and 15+ bias-mitigation algorithms spanning the entire ML lifecycle (pre-processing, in-processing, post-processing). Available in both Python and R.
- **Why it matters for the podcast:** AIF360 is one of the most complete fairness toolkits in existence, but its scope is also part of the critique — the sheer number of metrics and algorithms can overwhelm users. The project's *responsibility* to make these tools accessible to non-experts while maintaining scientific rigor is a recurring theme in its issue threads. The [documentation accuracy debate](https://github.com/Trusted-AI/AIF360/issues/528) over whether the `average_odds_difference` metric is correctly described as an "equalized odds relaxation" shows that even the *definitions* in the docs can mislead practitioners.
- **Get started:** `pip install aif360` | [Interactive demo](https://aif360.res.ibm.com/data) | [Documentation](https://aif360.readthedocs.io/)

### 3. [Aequitas](https://github.com/dssg/aequitas) — `dssg/aequitas`
- **Stars:** 773 | **Language:** Python | **License:** BSD-3-Clause
- **What it does:** A bias audit toolkit from the University of Chicago's Data Science for Social Good lab. Focuses on confusion-matrix-based fairness auditing with an interactive web app (Ridgeline plots, slice statistics).
- **Why it matters for the podcast:** Aequitas was built by social scientists and engineers *together* — its design reflects a deliberate choice to make fairness auditing accessible to people who aren't ML experts. The tool's architecture embodies the question: *should fairness tools be built for practitioners or for communities?*
- **Get started:** [Gallery](https://aequitas.dssg.io/gallery/) | [Docs](https://aequitas.readthedocs.io/)

---

## 🔬 Tier 2 — Specialized & Research Tools

### 4. [Fair-Code](https://github.com/yakew7/Fair-Code) — `yakew7/Fair-Code`
- **Stars:** 47 | **Language:** HTML/Python
- **What it does:** A collection of 7 open-source audits for algorithmic bias across criminal justice, hiring, lending, healthcare, welfare, and tenant screening. Each audit comes with measurable fairness goals and real-world impact analysis.
- **Why it matters for the podcast:** This project is notable for its *domain specificity* — it doesn't try to be a general-purpose toolkit but instead provides deep, contextual audits for high-stakes decision domains. It embodies the argument that fairness tools should be built *with* the communities they affect, not *for* them from the outside.

### 5. [Responsibly](https://github.com/ResponsiblyAI/responsibly) — `ResponsiblyAI/responsibly`
- **Stars:** 100 | **Language:** Python | **License:** Apache-2.0
- **What it does:** A Python-first auditing toolkit aligned with the textbook *Fairness and Machine Learning* (Barocas, Hardt & Narayanan). Includes word-embedded bias metrics, image-based fairness metrics, and causal fairness analysis.
- **Why it matters for the podcast:** Responsibly bridges the gap between academic fairness research and practical auditing. Its alignment with a canonical textbook gives it academic credibility, while its Python-first design makes it accessible to working practitioners.

---

## 🏗️ Tier 3 — Emerging & Niche Tools

### 6. [LiFT](https://github.com/linkedin/LiFT) — `linkedin/LiFT`
- **Stars:** 173 | **Language:** Scala | **License:** Apache-2.0
- **What it does:** LinkedIn's toolkit for measuring fairness at web scale using permutation testing. Designed for large-scale recommendation and search systems where traditional fairness metrics don't apply directly.
- **Why it matters for the podcast:** LiFT demonstrates that fairness measurement at *web scale* creates entirely different challenges than fairness measurement in a single model. The "fairness of a recommendation system" is a fundamentally different question than "fairness of a hiring model."

---

## 🗺️ How to Use This Resource

| If you want to... | Start here |
|-------------------|------------|
| Understand the core fairness toolkit landscape | Read the Tier 1 descriptions above |
| See fairness auditing in a specific domain | Look at Fair-Code (criminal justice, hiring, lending, etc.) |
| Explore fairness at scale | Check out LiFT (LinkedIn's web-scale approach) |
| Find the academic-practitioner bridge | Browse Responsibly |
| Dive into the live debates | See [DEBATES.md](./DEBATES.md) and the [GitHub Issues](https://github.com/bro26man-hash/ai-ethics-podcast-resources/issues) in this repo |

---

## 📊 At a Glance

| Project | Stars | License | Primary Focus | Key Tension |
|---------|-------|---------|---------------|-------------|
| AIF360 | 2,866 | Apache 2.0 | Comprehensiveness | Breadth vs. depth — does covering 70+ metrics dilute rigor? |
| Fairlearn | 2,249 | MIT | Accessibility | Simplicity vs. honesty — can simplified docs convey fundamental impossibility results? |
| Aequitas | 773 | MIT | Public-sector impact | Generalizability vs. specificity — should fairness tools differ for government vs. industry? |
| Fair-Code | 47 | — | Domain specificity | Specialization vs. generalization — can deep audits coexist with general toolkits? |
| Responsibly | 100 | Apache 2.0 | Academic-practitioner bridge | Theory vs. practice — how much scholarly grounding does a practitioner need? |
| LiFT | 173 | Apache 2.0 | Web-scale fairness | Context — is fairness at scale a fundamentally different problem? |

---

*These projects represent a snapshot of the open-source fairness ecosystem as of September 2026. The field evolves rapidly — if you find a project that should be included (or one that's no longer active), [open an issue](https://github.com/bro26man-hash/ai-ethics-podcast-resources/issues/new) and let us know.*
