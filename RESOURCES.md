# 🎙️ AI Ethics & Social Justice — Curated Resource Links

A crowdsourced collection of the most important open-source projects, tools, and reading materials for our podcast on technology and social justice.

---

## 📦 Core Open-Source Fairness & Bias-Auditing Toolkits

### 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Stars:** ⭐ 2,866 | **Language:** Python & R | **License:** Apache-2.0
- **What it is:** An extensible open-source toolkit from IBM Research providing a comprehensive set of fairness metrics for datasets and ML models, explanations for those metrics, and algorithms to mitigate bias throughout the AI lifecycle.
- **Key metrics:** Disparate Impact Ratio, Statistical Parity Difference, Equalized Odds, Average Odds Difference, Generalized Entropy Index, Differential Fairness
- **Key mitigation algorithms:** Preprocessing (Reweighing, Optimized Preprocessing), In-processing (Adversarial Debiasing, Fairness-constrained Classification), Post-processing (Equalized Odds Postprocessing, Calibrated Equalized Odds)
- **Why it matters for the podcast:** AIF360 is the *de facto* standard for fairness auditing in industry and academia. Its metric definitions shape how organizations measure bias — which makes the ongoing debate about whether `average_odds_difference` is correctly specified (see [Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)) a pivotal moment for the entire field.
- **Website:** https://aif360.res.ibm.com/
- **Slack:** [Join the AIF360 community](https://aif360.slack.com)

### 2. [Microsoft Fairlearn](https://github.com/fairlearn/fairlearn)
- **Stars:** ⭐ 2,286 | **Language:** Python | **License:** MIT
- **What it is:** A Python package that empowers developers to assess and improve fairness of AI systems. Fairlearn takes a explicitly human-centered approach, framing fairness in terms of *harms* (allocation harms and quality-of-service harms) rather than abstract statistical quantities.
- **Key approach:** Group fairness with explicitly specified social groups — fairness constraints are treated as trade-offs that humans must evaluate
- **Why it matters for the podcast:** Fairlearn's philosophy — that "fairness is fundamentally a sociotechnical challenge" and that "many quantitative fairness metrics cannot all be satisfied simultaneously" — directly addresses the question of *who a tool should serve*. Their user guide forces practitioners to choose which fairness definition applies to their context, rather than offering a one-size-fits-all solution.
- **Website:** https://fairlearn.org/
- **Discord:** [Join the Fairlearn Discord](https://discord.gg/R22yCfgsRn)

### 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)
- **Stars:** ⭐ 310 | **Language:** Python | **License:** MIT
- **What it is:** A comprehensive enterprise-grade toolkit covering safety, security, privacy, explainability, fairness, bias detection, and hallucination detection for LLMs and traditional ML models.
- **Key fairness features:** Statistical Parity Difference, Disparate Impact Ratio, Four-Fifths Rule, Cohen's D for bias detection; Equalized Odds and Re-weighing for mitigation
- **Why it matters for the podcast:** Represents the *industrial* perspective on responsible AI — how large organizations operationalize fairness at scale. Its modular micro-frontend architecture shows how fairness auditing can be embedded into production AI pipelines, not just research notebooks.
- **Docs:** https://infosys.github.io/Infosys-Responsible-AI-Toolkit/

---

## 📚 Key Papers & Guides Referenced by These Projects

| Paper | Authors | Year | Core Question |
|-------|---------|------|---------------|
| [Inherent Trade-Offs in the Fair Determination of Risk Scores](https://arxiv.org/abs/1609.05807) | Kleinberg, Mullainathan, Raghavan | 2016 | *Can we satisfy all fairness definitions simultaneously?* |
| [Equality of Opportunity in Supervised Learning](https://papers.nips.cc/paper/6374-equality-of-opportunity-in-supervised-learning) | Hardt, Price, Srebro | 2016 | *Should we equalize prediction accuracy across groups?* |
| [Fairness Beyond Disparate Treatment & Disparate Impact](https://arxiv.org/abs/1703.06403) | Binns | 2017 | *What does justice require from algorithmic systems?* |
| [On the Misuse of Fairness Metrics](https://arxiv.org/abs/2309.12649) | Abdollahpouri et al. | 2021 | *Are popular fairness metrics actually measuring what we think?* |
| [The Mathematical Mechanization of Injustice](https://www.colorofviolence.org/the-mathematical-mechanization-of-injustice/) | Benjamin | 2019 | *How does technology inherit and amplify structural racism?* |

---

## 🎧 Podcast Episode Planning Resources

- **Episode focus:** How the metrics we use to detect bias shape the biases we fail to see
- **Central tension:** AIF360's `average_odds_difference` metric has been flagged as mathematically incorrectly specified as an equalized odds relaxation — yet it remains in production documentation and IBM's public-facing fairness guides. What does it mean for communities relying on these tools when the measurements themselves are contested?
- **Discussion threads to follow:**
  - [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — Metric definition controversy
  - [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Extension of Empirical Differential Fairness
- **Community voices:** AIF360 Slack, Fairlearn Discord, ACM FAccT conference proceedings

---

*This is a living document. To suggest additions or corrections, open an issue or submit a pull request.*