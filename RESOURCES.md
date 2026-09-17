# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of active open-source projects related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects

### 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360) — *Fairness Metrics Library & Bias Mitigation*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |
| **Maintainers** | IBM Trustworthy AI

**What it does:** A comprehensive toolkit providing (1) a library of **fairness metrics** for datasets and models, (2) **explanations** for each metric, and (3) **algorithms to mitigate bias** — covering pre-processing, in-processing, and post-processing approaches. Supports binary and continuous protected attributes, and includes guidelines for fair machine learning.

**Why it matters for the podcast:** AIF360 is one of the most-cited fairness toolkits in academia and industry. Its metric definitions are the de facto standard for fairness auditing in many organizations. But as **Issue #528** reveals, even widely-used metric definitions can be mathematically mischaracterized in documentation — raising questions about who audits the auditors.

**Key metric categories:**
- **Group fairness metrics:** Disparate Impact, Statistical Parity Difference, Mean Difference
- **Individual fairness metrics:** Total Variation Distance
- **Classification fairness metrics:** Average Odds Difference, Equalized Odds Difference, Disparate Impact Remover

**Run it locally:**
```bash
pip install aif360
python -c "from aif360.metrics import ClassificationMetric; print('get started')"
```

**Podcast angle:** The `average_odds_difference` documentation bug (Issue #528) is a perfect case study — a metric used by thousands of practitioners was misdescribed for years. What does it mean when the tools meant to catch bias contain the very kind of definitional error they're supposed to prevent?

---

### 2. [Microsoft Fairlearn](https://github.com/fairlearn/fairlearn) — *Fairness Assessment & Mitigation for ML*

| | |
|---|---|
| **Stars** | 2,286 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |
| **Maintainers** | Fairlearn community (Microsoft)

**What it does:** A Python package that empowers developers to **assess** their AI system's fairness and **mitigate** observed unfairness. Contains both metrics for model evaluation and mitigation algorithms (Reductions, Post-processing, Pre-processing). Emphasizes **group fairness** and the distinction between *allocation harms* (hiring, admissions, lending) and *quality-of-service harms*.

**Why it matters for the podcast:** Fairlearn's README makes a philosophically rich claim: *"Fairness is fundamentally a sociotechnical challenge. Many aspects of fairness, such as justice and due process, are not captured by quantitative fairness metrics."* This self-awareness about the limits of quantification is exactly the tension the podcast explores.

**Key features:**
- **Assessment dashboard** for comparing models across fairness & accuracy trade-offs
- **Mitigation algorithms:** ExponentiatedGradient, ThresholdOptimizer, RejectOptionClassification
- **Connectivity to scikit-learn** pipelines

**Run it locally:**
```bash
pip install fairlearn
pip install fairlearn[notebooks]  # example notebooks
```

**Podcast angle:** Fairlearn explicitly acknowledges that fairness metrics can't capture justice or due process — but then ships tools that treat them as if they can. That gap is where the real policy debate lives.

---

### 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — *Enterprise AI Governance & Bias Detection*

| | |
|---|---|
| **Stars** | 310 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |

**What it does:** An enterprise-grade toolkit covering **safety, security, explainability, fairness, bias detection, and hallucination detection** for LLMs and traditional ML. Designed for organizational AI governance — not just research, but compliance-grade auditing.

**Why it matters for the podcast:** Represents the "governance-first" school of thought — fairness as an organizational process, not just a technical metric. Where AIF360 and Fairlearn serve researchers and practitioners, the Infosys toolkit targets **compliance officers and AI ethics boards**.

**Podcast angle:** The shift from "is this model fair?" (research question) to "can you prove it, with an audit trail?" (governance question) is itself a power dynamic — who gets to define and enforce "fair" in an organization?

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Focus | Maturity | Best For |
|---|---|---|---|
| **AIF360** | Metric library + mitigation algorithms | Production-grade, widely cited | Researchers, auditors needing standard fairness metrics |
| **Fairlearn** | Assessment + mitigation with scikit-learn integration | Production-grade, well-documented | Practitioners building fairness into ML pipelines |
| **Infosys RAI** | Enterprise governance + compliance | Enterprise alpha, governance-oriented | AI ethics boards, compliance, regulatory mapping |

---

## 📖 Key Fairness Concepts (for Podcast Preparation)

| Concept | Definition | Tension |
|---|---|---|
| **Demographic Parity** | Outcome rates should be equal across groups | Ignores legitimate differences in qualification |
| **Equalized Odds** | TPR and FPR should be equal across groups | Cannot coexist with predictive parity when base rates differ |
| **Predictive Parity** | PPV should be equal across groups | May require different FPRs per group |
| **Counterfactual Fairness** | Prediction should be same if protected attribute were different | Requires contested causal inferences |

---

## 🔗 External Resources

- [AIF360 Documentation](https://aif360.readthedocs.io/) — IBM's official metric & algorithm docs
- [Fairlearn User Guide](https://fairlearn.org/main/user_guide/index.html) — Microsoft's fairness assessment guide
- [ Infosys Responsible AI Toolkit README](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit/blob/main/README.md)
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — Impossibility theorem
- [Kleinberg, Mullainathan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Parallel impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper

---

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.以内内容以符合知识共享方式贡献。*
