# 🎙️ AI Ethics & Social Justice Podcast — Resource Hub

A curated, crowdsourced collection of open-source projects, tools, and readings at the intersection of AI ethics, algorithmic fairness, and bias auditing.

---

## 🔧 Key Open-Source Projects

### 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Stars:** 2,865 | **Language:** Python / R | **License:** Apache-2.0
- **Maintainer:** IBM Research (Trusted-AI org)
- **What it does:** A comprehensive toolkit with 70+ fairness metrics and 15+ bias-mitigation algorithms covering the entire ML lifecycle — preprocessing, in-processing, post-processing. Includes interactive notebooks, R interface, and explanatory materials.
- **Why it matters:** One of the earliest and most cited fairness toolkits. Its breadth illustrates how hard it is to operationalize "fairness" — different metrics often disagree, and choosing one over another is itself a value judgment. The tool's own internal categorization of metrics is the subject of a fierce unresolved debate (see Debate #2 in DEBATES.md).

### 2. [Microsoft Fairlearn](https://github.com/fairlearn/fairlearn)
- **Stars:** 2,285 | **Language:** Python | **License:** MIT
- **Maintainer:** Microsoft (Fairlearn project under the .NET Foundation)
- **What it does:** A Python package to assess and improve fairness of ML models. Provides metrics like demographic parity, equalized odds, and predictive rate parity, plus mitigation algorithms from pre-processing to post-processing. Core feature is the `MetricFrame` for disaggregated evaluation across sensitive groups. Explicitly frames fairness as a *sociotechnical* challenge — not just a mathematical one.
- **Why it matters:** Fairlearn's `MetricFrame` is the go-to tool for disaggregated evaluation. Its governance model and ongoing API debates make it a case study in how fairness tools are shaped by community discourse — and who gets left out of the design conversation. The maintainers are currently wrestling with whether the tool should extend to non-human domains of harm (see Debate #3 below).

### 3. [Ubuntu-AI-Bias-Auditing](https://github.com/sinqobiledube/Ubuntu-AI-Bias-Auditing) — *Ubuntu Decolonial Framework*
- **Stars:** 0 (emerging) | **Language:** Python | **License:** None (research prototype)
- **Maintainer:** Sinqobile Dube (Pretoria University), with collaborators M. Mguni and S. S. Dube (2025 ICAT paper)
- **What it does:** A fairness framework that integrates Ubuntu philosophy ("I am because we are") and decolonial principles into the entire ML pipeline. Provides World Bank data ingestion, fairness diagnostics (Disparate Impact Ratio, Equalized Odds Difference, Demographic Parity Difference), intersectional subgroup analysis, statistical validation (ANOVA proving non-additive intersectional harm), and a Tier 1-3 mitigation pipeline (Sankofa null-space projection → multi-objective optimization → Ubuntu-based Pareto governance with human-in-the-loop model selection). Includes a Streamlit dashboard.
- **Why it matters:** This is the only known fairness toolkit that approaches bias auditing from an explicit *decolonial* perspective — as opposed to the Eurocentric, individual-rights frameworks embedded in Western fairness mathematics. It raises the question central to our podcast: **whose theory of justice does "fairness" encode?** When AIF360 measures "disparate impact" and Fairlearn measures "demographic parity," both assume a specific Western-liberal framework of individual equality. The Ubuntu framework asks whether that framework is itself a form of epistemic colonialism — and whether fairness for Sub-Saharan African communities requires fundamentally different metrics and different definitions of harm.

---

## 📂 Additional Notable Projects

| Project | Stars | Description |
|---------|-------|-------------|
| **Themis-ML** | 126 | Fairness-aware ML algorithms (massaging, reject option classification) built on pandas/sklearn. Explicitly frames fairness as the inverse of discrimination. | [cosmicBboy/themis-ml](https://github.com/cosmicBboy/themis-ml) |
| **Infosys Responsible AI Toolkit** | 300+ | Modular toolkit covering fairness, bias detection, privacy, safety, security for both LLMs and traditional ML | [Infosys/Infosys-Responsible-AI-Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) |
| **LiFT** | 173 | LinkedIn's Scala/Spark toolkit for web-scale fairness measurement with permutation testing | [linkedin/LiFT](https://github.com/linkedin/LiFT) |
| **Aequitas** | 772 | University of Chicago toolkit for confusion-matrix-based fairness auditing with interactive API | [dssg/aequitas](https://github.com/dssg/aequitas) |
| **Fair-Code** | 46 | Seven open-source algorithmic audits across criminal justice, hiring, lending, healthcare, welfare, tenant screening | [yakew7496/Fair-Code](https://github.com/yakew7496/Fair-Code) |

---

## 🔬 How These Tools Relate to Social Justice

These toolkits sit at the intersection of technology and justice:

- **Who gets audited?** Most tools assume a classification/regression setting — but the communities most harmed by algorithmic decisions (e.g., predictive policing, welfare eligibility) often operate in settings these tools don't serve.
- **Who defines fairness?** Each toolkit implements specific fairness definitions (demographic parity, equalized odds, predictive parity…) — but these definitions *disagree with each other*, and choosing one is a political act, not a neutral technical decision. As one AIF360 contributor wrote in a 2019 issue, *"What is 'fair' & 'correct' is highly situational. What is 'fair' in one situation may not be 'fair' in another."*
- **Who benefits?** Fairness tools are primarily used by technologists and companies building AI systems. The communities most affected by algorithmic harm rarely have the technical literacy to audit these systems themselves. Aequitas tries to bridge this gap with plain-language audit reports, but the fundamental tension remains: the tool's definition of fairness may not match the community's definition of justice.
- **Whose philosophy is embedded?** The Ubuntu Decolonial Framework challenges the entire foundation: the math of fairness itself (disparate impact, demographic parity, equalized odds) is built on Western-liberal assumptions about individual rights and group equality. Ubuntu philosophy — rooted in communal interdependence and relational personhood — offers a fundamentally different starting point. Which framework deserves to be called "fairness" is not a technical question. It is a philosophical and political one.

---

## 📚 Suggested Reading & Listening

| Resource | Type | Description |
|----------|------|-------------|
| [Fairness and Machine Learning](https://fairmlbook.org) | Book | Barocas, Hardt & Narayanan — the foundational textbook on algorithmic fairness |
| [AI Fairness 360 Interactive](https://aif360.res.ibm.com/data) | Interactive | IBM's hands-on introduction to fairness concepts |
| [Aequitas Tutorial](https://dssg.github.io/fairness_tutorial/) | Tutorial | Deep dive into fairness auditing from the University of Chicago |
| [Dube et al. (2025) — From Universalism to Ubuntu](https://ieeexplore.ieee.org/document/10867937) | Paper | The decolonial fairness framework at the heart of the Ubuntu-AI-Bias-Auditing project |
| [Hagendorff et al. (2023) — Speciesist Bias in AI](https://doi.org/10.1007/s43681-023-00380-w) | Paper | Documented speciesist bias in GPT-3; calls for fairness frameworks to include non-human animals |

---

## 🤝 Contributing

This is a living resource! To suggest additions:
1. Open an issue describing the resource and why it's relevant.
2. Fork this repo, add your resource to `RESOURCES.md`, and open a PR.
3. Join the discussion on `DEBATES.md` — we need diverse perspectives.

---

## 🎧 About the Podcast

This hub supports a podcast exploring the human side of algorithmic systems — how they encode bias, who they serve, and what "fair" really means. Every episode draws on real debates happening *right now* in open-source fairness communities.