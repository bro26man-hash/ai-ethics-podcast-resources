# 🎙️ AI Ethics & Social Justice Podcast — Resource Hub

A curated, crowdsourced collection of open-source projects, tools, and readings at the intersection of AI ethics, algorithmic fairness, and bias auditing.

---

## 🔧 Key Open-Source Projects

### 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Stars:** 2,866 | **Language:** Python / R | **License:** Apache-2.0
- **Maintainer:** IBM Research (Trusted-AI org)
- **What it does:** A comprehensive toolkit with 70+ fairness metrics and 15+ bias-mitigation algorithms covering the entire ML lifecycle — preprocessing, in-processing, post-processing. Includes interactive notebooks, R interface, and explanatory materials.
- **Why it matters:** One of the earliest and most cited fairness toolkits. Its breadth illustrates how hard it is to operationalize "fairness" — different metrics often disagree, and choosing one over another is itself a value judgment. The tool's own internal categorization of metrics is the subject of fierce unresolved debates (see Debate #2 and Debate #5 in DEBATES.md). Ongoing issues include a proposal to extend the Empirical Differential Fairness metric to return intersectional group-pair breakdowns (#558), a long-standing debate about whether the website is maintained (#548), and a fundamental challenge to whether its "average odds difference" metric actually measures what it claims (#528).

### 2. [Microsoft Fairlearn](https://github.com/fairlearn/fairlearn)
- **Stars:** 2,285 | **Language:** Python | **License:** MIT
- **Maintainer:** Microsoft (Fairlearn project under the .NET Foundation)
- **What it does:** A Python package to assess and improve fairness of ML models. Provides metrics like demographic parity, equalized odds, and predictive rate parity, plus mitigation algorithms from pre-processing to post-processing. Core feature is the `MetricFrame` for disaggregated evaluation across sensitive groups. Explicitly frames fairness as a *sociotechnical* challenge — not just a mathematical one.
- **Why it matters:** Fairlearn's `MetricFrame` is the go-to tool for disaggregated evaluation. Its governance model and ongoing API debates make it a case study in how fairness tools are shaped by community discourse — and who gets left out of the design conversation. Active issues include a philosophical debate about whether species should be a recognized sensitive feature (#1625) and a technical inconsistency between MetricFrame and plot_roc_curve_by_group on missing sensitive values (#1725). (See Debate #1, #3, and #4 in DEBATES.md.)

### 3. [Responsibly](https://github.com/ResponsiblyAI/responsibly)
- **Stars:** 101 | **Language:** Python | **License:** MIT
- **Maintainer:** ResponsiblyAI (independent open-source collective)
- **What it does:** A Python-first auditing toolkit aligned with the book *Fairness and Machine Learning* (Barocas, Hardt & Narayanan). Three sub-packages: `dataset` (benchmark datasets), `fairness` (demographic fairness in binary classification with metrics and algorithmic interventions), and `we` (word-embedded bias metrics and debiasing methods for NLP). Designed for practitioners and researchers familiar with scikit-learn, Numpy, and Pandas.
- **Why it matters:** Responsibly fills an important gap between the heavyweight toolkits (AIF360's sheer breadth, Fairlearn's MetricFrame) and the need for a lightweight, scikit-learn-compatible workflow. Its NLP focus (the `we` module for word-embedding bias) makes it one of the few fairness tools that explicitly addresses language-model bias — the very domain where much contemporary algorithmic harm occurs. Its alignment with the Barocas/Hardt/Narayanan textbook means its fairness definitions inherit the philosophical tensions those authors themselves acknowledge: multiple incommensurable fairness definitions, no neutral choice. (See Debate #3 in DEBATES.md for how fairness tooling struggles with scope boundaries.)

### 4. [Aequitas](https://github.com/dssg/aequitas)
- **Stars:** 773 | **Language:** Python | **License:** MIT
- **Maintainer:** University of Chicago (Data Science & Public Policy Group — Dr. Rayid Ghani's lab)
- **What it does:** An open-source bias auditing and Fair ML toolkit designed for data scientists, researchers, and policymakers. Provides confusion-matrix-based fairness metrics (TPR, FPR, PPV, etc.) per sensitive group, interactive visualization, and a "Flow" module for experimenting with bias mitigation methods (pre-processing, in-processing, post-processing). Ships with two built-in datasets (BankAccountFraud, FolkTables) and supports custom methods via intuitive interfaces.
- **Why it matters:** Aequitas bridges the gap between technical fairness tooling and practical auditing — its interactive API and plain-language reports make it accessible to non-engineers, including policymakers and advocacy organizations. The University of Chicago lineage connects it to the *Aequitas* tradition of "justice as fairness" in the Rawlsian sense, raising questions about whether the toolkit's Western-liberal foundations are appropriate for auditing systems that affect communities with different philosophical traditions. (See Debate #4 in DEBATES.md for the metric accuracy controversy that spans both AIF360 and the broader fairness tooling ecosystem.)

---

## 📂 Additional Notable Projects

| Project | Stars | Description | Link |
|---------|-------|-------------|------|
| **Fair-Code** | 46 | Seven open-source algorithmic audits across criminal justice, hiring, lending, healthcare, welfare, tenant screening — with measurable fairness goals | [yakew7496/Fair-Code](https://github.com/yakew7496/Fair-Code) |
| **EqualityML** | 35 | Evidence-based tools and community collaboration to end algorithmic bias, one data scientist at a time | [EqualityAI/EqualityML](https://github.com/EqualityAI/EqualityML) |
| **Themis-ML** | 126 | Fairness-aware ML algorithms (massaging, reject option classification) built on pandas/sklearn. Explicitly frames fairness as the inverse of discrimination | [cosmicBboy/themis-ml](https://github.com/cosmicBboy/themis-ml) |
| **Infosys Responsible AI Toolkit** | 300+ | Modular toolkit covering fairness, bias detection, privacy, safety, security for both LLMs and traditional ML | [Infosys/Infosys-Responsible-AI-Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) |
| **LiFT** | 173 | LinkedIn's Scala/Spark toolkit for web-scale fairness measurement with permutation testing | [linkedin/LiFT](https://github.com/linkedin/LiFT) |

---

## 🔬 How These Tools Relate to Social Justice

These toolkits sit at the intersection of technology and justice:

- **Who gets audited?** Most tools assume a classification/regression setting — but the communities most harmed by algorithmic decisions (e.g., predictive policing, welfare eligibility) often operate in settings these tools don't serve.
- **Who defines fairness?** Each toolkit implements specific fairness definitions (demographic parity, equalized odds, predictive parity…) — but these definitions *disagree with each other*, and choosing one is a political act, not a neutral technical decision. As one AIF360 contributor wrote in a 2019 issue, *"What is 'fair' & 'correct' is highly situational. What is 'fair' in one situation may not be 'fair' in another."*
- **Who benefits?** Fairness tools are primarily used by technologists and companies building AI systems. The communities most affected by algorithmic harm rarely have the technical literacy to audit these systems themselves. Aequitas tries to bridge this gap with plain-language audit reports, but the fundamental tension remains: the tool's definition of fairness may not match the community's definition of justice.
- **Whose philosophy is embedded?** The Ubuntu Decolonial Framework (featured in the Ubuntu-AI-Bias-Auditing project) challenges the entire foundation: the math of fairness itself (disparate impact, demographic parity, equalized odds) is built on Western-liberal assumptions about individual rights and group equality. Ubuntu philosophy — rooted in communal interdependence and relational personhood — offers a fundamentally different starting point. Which framework deserves to be called "fairness" is not a technical question. It is a philosophical and political one.

---

## 📚 Suggested Reading & Listening

| Resource | Type | Description |
|----------|------|-------------|
| [Fairness and Machine Learning](https://fairmlbook.org) | Book | Barocas, Hardt & Narayanan — the foundational textbook on algorithmic fairness |
| [AI Fairness 360 Interactive](https://aif360.res.ibm.com/data) | Interactive | IBM's hands-on introduction to fairness concepts |
| [Aequitas Tutorial](https://dssg.github.io/fairness_tutorial/) | Tutorial | Deep dive into fairness auditing from the University of Chicago |
| [Aequitas Flow Paper (JMLR 2024)](https://jmlr.org/papers/v25/24-0677.html) | Paper | Jesús et al. — "Aequitas Flow: Streamlining Fair ML Experimentation" |
| [Dube et al. (2025) — From Universalism to Ubuntu](https://ieeexplore.ieee.org/document/10867937) | Paper | The decolonial fairness framework at the heart of the Ubuntu-AI-Bias-Auditing project |
| [Hagendorff et al. (2023) — Speciesist Bias in AI](https://doi.org/10.1007/s43681-023-00380-w) | Paper | Documented speciesist bias in GPT-3; calls for fairness frameworks to include non-human animals |
| [Responsibly Documentation](https://docs.responsibly.ai) | Docs | Full docs for the Responsibly toolkit, including fairness metrics and word-embedding bias |
| [Fairlearn User Guide](https://fairlearn.readthedocs.io) | Docs | Microsoft's Fairlearn documentation, including MetricFrame API and stakeholder identification |

---

## 🤝 Contributing

This is a living resource! To suggest additions:
1. Open an issue describing the resource and why it's relevant.
2. Fork this repo, add your resource to `RESOURCES.md`, and open a PR.
3. Join the discussion on `DEBATES.md` — we need diverse perspectives.

---

## 🎧 About the Podcast

This hub supports a podcast exploring the human side of algorithmic systems — how they encode bias, who they serve, and what "fair" really means. Every episode draws on real debates happening *right now* in open-source fairness communities.
