# 🎙️ AI Ethics & Social Justice Podcast — Resource Hub

A curated, crowdsourced collection of open-source projects, tools, and readings at the intersection of AI ethics, algorithmic fairness, and bias auditing.

---

## 🔧 Key Open-Source Projects

### 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Stars:** 2,865 | **Language:** Python / R | **License:** Apache-2.0
- **Maintainer:** IBM Research (Trusted-AI org)
- **What it does:** A comprehensive toolkit with 70+ fairness metrics and 15+ bias-mitigation algorithms covering the entire ML lifecycle — preprocessing, in-processing, post-processing. Includes interactive notebooks and explanatory materials.
- **Why it matters:** One of the earliest and most cited fairness toolkits. Its breadth illustrates how hard it is to operationalize "fairness" — different metrics often disagree, and choosing one over another is itself a value judgment. The tool's own internal categorization of metrics is the subject of a fierce unresolved debate (see Debate #2 in DEBATES.md).
- **Key debates:**
  - [Issue #214 — What do "inequality indices" actually measure? Fairness or utility?](https://github.com/Trusted-AI/AIF360/pull/214) (9 comments, open since 2020)
  - [Issue #97 — Can a tool "remove bias"? Reconsidering the word "bias" in the README](https://github.com/Trusted-AI/AIF360/issues/97) (philosophical)

### 2. [Microsoft Fairlearn](https://github.com/fairlearn/fairlearn)
- **Stars:** 2,200+ | **Language:** Python
- **What it does:** A Python package to assess and improve the fairness of machine learning models. Provides metrics like demographic parity, equalized odds, and predictive rate parity — plus mitigation algorithms ranging from pre-processing to post-processing. Core feature is the `MetricFrame` for disaggregated evaluation.
- **Why it matters:** Fairlearn's `MetricFrame` is the go-to tool for disaggregated evaluation. Its governance model and ongoing API debates make it a case study in how fairness tools are shaped by community discourse — and who gets left out of the design conversation.
- **Key debate:** [#756 — MetricFrame should support metrics that don't require y_true and y_pred](https://github.com/fairlearn/fairlearn/issues/756) (74 comments, open)

### 3. [Aequitas](https://github.com/dssg/aequitas)
- **Stars:** 773 | **Language:** Python | **License:** MIT
- **Maintainer:** University of Chicago (Data Science & Public Policy group)
- **What it does:** An open-source bias auditing and Fair ML toolkit for data scientists, ML researchers, and policymakers. Provides confusion-matrix-based fairness metrics (TPR, FPR, PPV, etc.) plus pre-, in-, and post-processing mitigation methods. Includes Aequitas Flow for streamlined experimentation.
- **Why it matters:** Aequitas is explicitly designed for non-technical stakeholders — its audit output is meant to be readable by policymakers and community advocates. But its documentation acknowledges a tension: the metrics it implements are mathematical definitions of fairness that may not align with the values of the communities being audited. This gap between "what the tool measures" and "what the community considers fair" is a core theme for our episode.
- **Key issue:** [#201 — Add documentation for existing fairness metrics](https://github.com/dssg/aequitas/issues/201) (open)

---

## 📂 Additional Notable Projects

| Project | Stars | Description | Link |
|---------|-------|-------------|------|
| **Themis-ML** | 126 | Fairness-aware ML algorithms (massaging, reject option classification) built on pandas/sklearn. Explicitly frames fairness as the inverse of discrimination. | [cosmicBboy/themis-ml](https://github.com/cosmicBboy/themis-ml) |
| **Infosys Responsible AI Toolkit** | 300+ | Modular toolkit covering fairness, bias detection, privacy, safety, security for both LLMs and traditional ML | [Infosys/Infosys-Responsible-AI-Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) |
| **LiFT** | 173 | LinkedIn's Scala/Spark toolkit for web-scale fairness measurement with permutation testing | [linkedin/LiFT](https://github.com/linkedin/LiFT) |
| **Fair-Code** | 46 | Seven open-source algorithmic audits across criminal justice, hiring, lending, healthcare, welfare, tenant screening | [yakew7496/Fair-Code](https://github.com/yakew7496/Fair-Code) |
| **AI Equity Toolkit** | — | UChicago's toolkit for equity-aware ML | [okkan/ai-equity-toolkit](https://github.com/okkan/ai-equity-toolkit) |
| **Racial Data Tool** | — | ProPublica's investigative tool for racial bias in algorithms | [propublica/racial-datatool](https://github.com/propublica/racial-datatool) |

---

## 🔬 How These Tools Relate to Social Justice

These toolkits sit at the intersection of technology and justice:

- **Who gets audited?** Most tools assume a classification/regression setting — but the communities most harmed by algorithmic decisions (e.g., predictive policing, welfare eligibility) often operate in settings these tools don't serve.
- **Who defines fairness?** Each toolkit implements specific fairness definitions (demographic parity, equalized odds, predictive parity…) — but these definitions *disagree with each other*, and choosing one is a political act, not a neutral technical decision. As one AIF360 contributor wrote in a 2019 issue, *"What is 'fair' & 'correct' is highly situational. What is 'fair' in one situation may not be 'fair' in another."*
- **Who benefits?** Fairness tools are primarily used by technologists and companies building AI systems. The communities most affected by algorithmic harm rarely have the technical literacy to audit these systems themselves. Aequitas tries to bridge this gap with plain-language audit reports, but the fundamental tension remains: the tool's definition of fairness may not match the community's definition of justice.

---

## 📚 Suggested Reading & Listening

| Resource | Type | Description |
|----------|------|-------------|
| [Fairness and Machine Learning](https://fairmlbook.org) | Book | Barocas, Hardt & Narayanan — the foundational textbook on algorithmic fairness |
| [AI Fairness 360 Interactive](https://aif360.res.ibm.com/data) | Interactive | IBM's hands-on introduction to fairness concepts |
| [Aequitas Tutorial](https://dssg.github.io/fairness_tutorial/) | Tutorial | Deep dive into fairness auditing from the University of Chicago |
| [Debate in AIF360 (#214)](https://github.com/Trusted-AI/AIF360/pull/214) | Live debate | Are "inequality indices" really fairness metrics, or just utility measures? |
| [Debate in Fairlearn (#756)](https://github.com/fairlearn/fairlearn/issues/756) | Live debate | Should MetricFrame support metrics without y_true/y_pred? |

---

## 🤝 Contributing

This is a living resource! To suggest additions:
1. Open an issue describing the resource and why it's relevant.
2. Fork this repo, add your resource to `RESOURCES.md`, and open a PR.
3. Join the discussion on `DEBATES.md` — we need diverse perspectives.

---

## 🎧 About the Podcast

This hub supports a podcast exploring the human side of algorithmic systems — how they encode bias, who they serve, and what "fair" really means. Every episode draws on real debates happening *right now* in open-source fairness communities.
