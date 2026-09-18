# 🛠️ Active Open-Source Fairness & Bias Auditing Projects

A curated, living list of the most active open-source projects working on algorithmic fairness, bias auditing, and ethical AI. Submitted by listeners and contributors of the *AI Ethics & Social Justice* podcast.

---

## Tier 1 — Comprehensive Fairness Toolkits

### 1. [Trusted-AI/AIF360](https://github.com/Trusted-AI/AIF360) — AI Fairness 360
- **Stars:** ⭐ 2,866 | **Forks:** 914 | **License:** Apache-2.0 | **Language:** Python (also R)
- **Maintained by:** IBM Research (Trusted-AI organization)
- **Last updated:** September 2026 — actively maintained
- **What it does:** The most comprehensive open-source toolkit for detecting and mitigating bias in ML models. Provides 20+ bias mitigation algorithms (preprocessing, in-processing, post-processing), a rich suite of fairness metrics (group fairness, sample distortion, differential fairness, bias scan), and interactive educational resources.
- **Why it matters for the podcast:** AIF360 is the toolkit that shaped the conversation around operationalizing fairness. Its metric definitions have been cited in policy papers, regulatory submissions, and court cases. When the field argues about *how* to measure fairness, the answer often traces back to AIF360.
- **Known open debates:** See [DEBATES.md](./DEBATES.md) for threads on mislabeled metrics (#528) and intersectional analysis gaps (#558).
- **Podcast angle:** The authority that isn't always right — when the most-cited toolkit gets its own metrics wrong.

### 2. [fairlearn/fairlearn](https://github.com/fairlearn/fairlearn) — Fairlearn
- **Stars:** ⭐ 2,286 | **Forks:** 516 | **License:** MIT | **Language:** Python
- **Maintained by:** Microsoft (Fairlearn organization)
- **Last updated:** September 2026 — actively maintained
- **What it does:** A Python package that empowers developers to assess and mitigate unfairness in AI systems. Focused on two categories of harm: *allocation harms* (withholding opportunities) and *quality-of-service harms* (uneven system performance). Provides both metrics and mitigation algorithms, with strong emphasis on group fairness definitions.
- **Why it matters for the podcast:** Fairlearn's design philosophy explicitly frames fairness as a sociotechnical challenge. Its maintainers have been at the forefront of arguing that no single fairness metric is sufficient — and that the choice of metric is a *value judgment*, not a technical one.
- **Known open debates:** See [DEBATES.md](./DEBATES.md) for the major thread on MetricFrame API design and who tools should serve (#756), and metric edge-case handling (#543).
- **Podcast angle:** The tool that doesn't know what it's for — 74 comments on a single API design question.

### 3. [dssg/aequitas](https://github.com/dssg/aequitas) — Aequitas
- **Stars:** ⭐ 773 | **Forks:** 125 | **License:** MIT | **Language:** Python
- **Maintained by:** University of Chicago (Data Science for Social Good lab)
- **Last updated:** September 2026 — actively maintained
- **What it does:** A bias auditing and fairness toolkit designed for practical, real-world deployment. Aequitas focuses on generating audit reports that are interpretable by non-technical stakeholders — judges, policymakers, community members. It supports group fairness metrics and provides visualizations that make disparities visible. Version 1.0.0 introduced "Aequitas Flow" for end-to-end bias audit-and-mitigate pipelines.
- **Why it matters for the podcast:** Aequitas bridges the gap between technical fairness metrics and community accountability. It was built with the explicit goal of making fairness audits accessible to people who aren't data scientists — a radical departure from the "move fast and query metrics" approach.
- **Podcast angle:** Who gets to see the audit? Aequitas asks this question by designing for non-technical audiences.
- **Known open debates:** See [DEBATES.md](./DEBATES.md) for the documentation gap issue (#201).

---

## Tier 2 — Emerging & Specialized Tools

| Project | Stars | Focus | Link |
|---------|-------|-------|------|
| [sinqobiledube/Ubuntu-AI-Bias-Auditing](https://github.com/sinqobiledube/Ubuntu-AI-Bias-Auditing) | — | Decolonial AI fairness framework integrating Ubuntu philosophy | [Link](https://github.com/sinqobiledube/Ubuntu-AI-Bias-Auditing) |
| [AmirhosseinHonardoust/Cognitivelens-AI-Human-Comparison](https://github.com/AmirhosseinHonardoust/Cognitivelens-AI-Human-Comparison) | 20 | Visual analytics for human vs. AI decision alignment | [Link](https://github.com/AmirhosseinHonardoust/Cognitivelens-AI-Human-Comparison) |
| [Sameekshakumar/aif360_virtual_lab](https://github.com/Sameekshakumar/aif360_virtual_lab) | — | Interactive web app for exploring AIF360 fairness concepts (React + FastAPI) | [Link](https://github.com/Sameekshakumar/aif360_virtual_lab) |

---

## 📚 Key Papers & Guides Referenced by These Projects

| Resource | Description | Link |
|----------|-------------|------|
| AIF360 Technical Paper (Bellamy et al., 2018) | The foundational paper defining the fairness toolkit landscape | [arXiv:1810.01943](https://arxiv.org/abs/1810.01943) |
| Aequitas Flow Paper (Jesus et al., 2024) | Streamlining Fair ML Experimentation | [JMLR 2024](http://jmlr.org/papers/v25/24-0677.html) |
| Fairlearn User Guide — Fairness of ML Systems | Conceptual framework for understanding fairness definitions | [fairlearn.org](https://fairlearn.org/main/user_guide/fairness_in_machine_learning.html) |
| AIF360 Interactive Experience | Gentle intro to fairness concepts and capabilities | [aif360.res.ibm.com](https://aif360.res.ibm.com/data) |
| Hardt et al. (2016) — Equality of Opportunity | The equalized odds postprocessing paper | [NeurIPS 2016](https://papers.nips.cc/paper/6374-equality-of-opportunity-in-supervised-learning) |
| Feldman et al. (2015) — Disparate Impact Remover | Foundational preprocessing debiasing paper | [FAT* 2015](https://doi.org/10.1145/2783258.2783311) |
| Calman et al. (2017) — Optimized Preprocessing | Preprocessing for discrimination prevention | [NeurIPS 2017](http://papers.nips.cc/paper/6988-optimized-preprocessing-for-discrimination-prevention) |

---

## 🤝 How to Contribute

1. **Found a new fairness tool?** Add it to this file with stars, license, and a one-line description
2. **Spotted a debate in an issue thread?** Summarize it in [DEBATES.md](./DEBATES.md)
3. **Want to suggest a project for removal?** Open an issue explaining why it's no longer active or relevant

---

*This file is crowdsourced. Every contributor adds a layer of accountability — just like the fairness tools these projects build.*