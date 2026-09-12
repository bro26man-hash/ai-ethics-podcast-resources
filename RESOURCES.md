# 🛠️ Fairness & Bias-Auditing Toolkits — Curated Resource List

A crowdsourced collection of active open-source projects working on algorithmic fairness, bias auditing, and responsible AI. These are the tools shaping how society measures and mitigates harm in automated decision-making.

---

## ⭐ Featured Projects

### 1. [Fairlearn](https://github.com/fairlearn/fairlearn)
- **Stars:** 2,286 | **Language:** Python | **License:** MIT
- **Maintainer:** Microsoft
- **What it does:** Provides a Python package to assess and improve fairness of machine learning models. Core feature is the `MetricFrame` — a disaggregated metric evaluator that lets you compute fairness metrics (demographic parity, equalized odds, etc.) across sensitive attribute groups.
- **Why it matters for our podcast:** Fairlearn explicitly frames fairness as a *sociotechnical* challenge — not just a technical one. Their API design decisions (how to measure fairness, what trade-offs to expose) are themselves political choices. The ongoing `MetricFrame` API debate (see DEBATES.md) is a perfect case study.
- **Key issues:** [#756 — MetricFrame should support metrics that don't require y_true and y_pred](https://github.com/fairlearn/fairlearn/issues/756) (74 comments, open)

### 2. [AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Stars:** 2,865 | **Language:** Python / R | **License:** Apache-2.0
- **Maintainer:** IBM Research (Trusted-AI org)
- **What it does:** A comprehensive toolkit with 70+ fairness metrics and 15+ bias-mitigation algorithms covering the entire ML lifecycle — preprocessing, in-processing, post-processing. Includes interactive notebooks and explanatory materials.
- **Why it matters for our podcast:** AIF360 is one of the earliest and most ambitious efforts to operationalize fairness. Its breadth (covering everything from disparate impact remover to adversarial debiasing) reflects a philosophy that fairness is a multi-dimensional problem — but the `DisparateImpactRemover` has long faced usability complaints (see [issue #241](https://github.com/Trusted-AI/AIF360/issues/241)).
- **Key issues:** [#241 — DisparateImpactRemover doesn't seem to be working](https://github.com/Trusted-AI/AIF360/issues/241) (8 comments, open since 2021)

### 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)
- **Stars:** 310 | **Language:** Python
- **Maintainer:** Infosys
- **What it does:** Incorporates features including safety, security, explainability, fairness, bias detection, and hallucination detection. A more recent entrant into the fairness tooling space.
- **Why it matters for our podcast:** Represents the wave of enterprise-backed fairness tools that bring corporate resources to open-source fairness work — and the tensions that come with it (corporate vs. community governance, breadth vs. depth).

---

## 📂 Additional Notable Projects

| Project | Stars | Description | Link |
|---------|-------|-------------|------|
| **Aequitas** | 772 | Bias auditing toolkit from University of Chicago — confusion-matrix-based fairness assessment | [dssg/aequitas](https://github.com/dssg/aequitas) |
| **LiFT** | 173 | LinkedIn's Scala/Spark toolkit for web-scale fairness measurement with permutation testing | [linkedin/LiFT](https://github.com/linkedin/LiFT) |
| **Fair-Code** | 46 | Seven open-source algorithmic audits across criminal justice, hiring, lending, healthcare, welfare, tenant screening | [yakew7496/Fair-Code](https://github.com/yakew7496/Fair-Code) |
| **AI Bias Dashboard** | — | Streamlit + Fairlearn dashboard for fairness visualization | [theashverse/ai-bias-dashboard](https://github.com/theashverse/ai-bias-dashboard) |

---

## 🔬 How These Tools Relate to Social Justice

These toolkits sit at the intersection of技术 (technology) and justice:

- **Who gets audited?** Most tools assume a classification/regression setting — but the communities most harmed by algorithmic decisions (e.g., predictive policing, welfare eligibility) often operate in settings these tools don't serve.
- **Who defines fairness?** Each toolkit implements specific fairness definitions (demographic parity, equalized odds, etc.) — but these definitions *disagree with each other*, and choosing one is a political act, not a neutral technical decision.
- **Who benefits?** Fairness tools are primarily used by technologists and companies building AI systems. The communities most affected by algorithmic harm rarely have the technical literacy to audit these systems themselves.

---

*This list is crowdsourced. Found a project we missed? Open an issue or send a PR!*