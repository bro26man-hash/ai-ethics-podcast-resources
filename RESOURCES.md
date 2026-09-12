# 🎙️ AI Ethics & Social Justice Podcast — Resource Hub

A curated, crowdsourced collection of open-source projects, tools, and readings at the intersection of AI ethics, algorithmic fairness, and bias auditing.

---

## 🔧 Key Open-Source Projects

### 1. [Microsoft Fairlearn](https://github.com/fairlearn/fairlearn)
- **Stars:** 2,200+ | **Language:** Python
- A Python package to assess and improve the fairness of machine learning models. Provides metrics like demographic parity, equalized odds, and predictive rate parity — plus mitigation algorithms ranging from pre-processing to post-processing.
- **Why it matters:** Fairlearn's `MetricFrame` is the go-to tool for disaggregated evaluation. Its governance model (-filled by a diverse group of maintainers) makes it a case study in how fairness tools are shaped by community debate.

### 2. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Stars:** 2,800+ | **Language:** Python
- A comprehensive toolkit from IBM Research offering 70+ fairness metrics and 11 bias-mitigation algorithms across three stages: pre-processing, in-processing, and post-processing.
- **Why it matters:** One of the earliest and most cited fairness toolkits. Its breadth of metrics illustrates how hard it is to operationalize "fairness" — different metrics often disagree, and choosing one over another is itself a value judgment.

### 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)
- **Stars:** 300+ | **Language:** Python
- A modular toolkit covering fairness, bias detection, privacy, safety, security, and explainability for both LLMs and traditional ML models.
- **Why it matters:** Represents the industry push to embed fairness into production AI pipelines, with specific metrics like Statistical Parity Difference, Disparate Impact Ratio, and Cohen's D.

---

## 📚 Further Reading & Tools

| Resource | Link | Focus |
|---|---|---|
| Fairlearn Docs | https://fairlearn.org/ | Usage guides, API reference, examples |
| AIF360 Online | https://aif360.mybluemix.net/ | Interactive demos of IBM's toolkit |
| Racial Data Tool | https://github.com/propublica/racial-datatool | ProPublica's investigative tool for racial bias in algorithms |
| Google What-If Tool | https://pair-code.github.io/what-if-tool/ | Interactive model exploration without code |
| AI Equity Toolkit | https://github.com/okkan/ai-equity-toolkit | UChicago's toolkit for equity-aware ML |

---

## 🤝 Contributing

This is a living resource! To suggest additions:
1. Open an issue describing the resource and why it's relevant.
2. Fork this repo, add your resource to `RESOURCES.md`, and open a PR.
3. Join the discussion on `DEBATES.md` — we need diverse perspectives.

---

## 🎧 About the Podcast

This hub supports a podcast exploring the human side of algorithmic systems — how they encode bias, who they serve, and what "fair" really means. Every episode draws on real debates happening *right now* in open-source fairness communities.
