# 🎙️ AI Ethics & Social Justice — Podcast Resource Hub

A **crowdsourced hub** for the *AI Ethics & Social Justice* podcast — curating open-source tools, live debates, and community resources on algorithmic fairness, bias auditing, and the ethics of automated decision-making.

## 🎯 What This Is

This repo is a living research companion for our podcast episodes. We track:

- **Active open-source projects** building fairness toolkits, bias auditors, and governance platforms
- **Real debates happening in those projects** — disagreements over how to measure fairness, who tools should serve, and what "fair" even means
- **Community discussion** — pull requests, issues, and explainers that surface the tensions between technical rigor and social justice

## 📂 Repository Structure

```
ai-ethics-podcast-resources/
├── RESOURCES.md    # Curated links to open-source fairness projects
├── DEBATES.md      # Summaries of ongoing controversies from issue threads
├── README.md       # This file
```

## 🔍 What You'll Find

### Active Fairness Toolkits
We track 2–3 active open-source projects per episode, spanning:
- **Bias auditing pipelines** — end-to-end fairness audits on real datasets (COMPAS, hiring, lending, healthcare)
- **Fairness metric libraries** — implementations of demographic parity, equalized odds, predictive parity, counterfactual fairness
- **Governance & compliance platforms** — evidence-grade AI assurance for regulatory frameworks (EU AI Act, NIST AI RMF)

### Live Debates
Each episode zooms in on a **specific ongoing disagreement** from an open issue thread:
- How should we measure the influence of race in a model? (SHAP-based vs. causal approaches)
- Which fairness metric should dominate when they conflict? (Demographic parity vs. equalized odds vs. predictive parity)
- Who bears the cost of a "fair" model — and who decides?
- Can counterfactual fairness ever be computed without a contested causal graph?

## 🤝 How to Contribute

We welcome contributions from everyone — podcasters, engineers, ethicists, and the just-curious:

1. **Found a great fairness toolkit?** Add it to `RESOURCES.md`
2. **Spotted a real debate in an issue thread?** Summarize it in `DEBATES.md`
3. **Want to discuss an episode topic?** Open an issue with the `discussion` label
4. **Have a counterargument?** Open a PR — fairness debates thrive on disagreement

## 📌 Featured Debate (See `DEBATES.md`)

Each episode centers on a **real, unresolved controversy** from the open-source community. Current featured debates include:

- **The SHAP Measurement Problem** — When auditing racial bias via SHAP values, does the denominator (top-5 features vs. all features) change the story? ([GitHub Issue #672](https://github.com/yakew7/Fair-Code/issues/672))
- **The Counterfactual Fairness Reversal** — A synthetic lending audit's claimed racial disparity flips direction when actually reproduced. What does it mean when the "expected" outcome contradicts the real one? ([GitHub Issue #654](https://github.com/yakew7/Fair-Code/issues/654))
- **The Impossibility Triangle** — Demographic parity, equalized odds, and predictive parity cannot all hold simultaneously. Which one should you pick, and who decides? ([Fair-Code Explainer](https://github.com/yakew7/Fair-Code/blob/main/explainers/fairness-metric-conflicts.md))

## 📬 Feedback & Discussion

- **Episode suggestions** — Open an issue with label `episode-suggestion`
- **Debate nominations** — Found a great fairness thread? Open an issue with label `debate-nomination`
- **General discussion** — Open any issue, tag it `discussion`

## 📜 License

This resource hub is released under the [MIT License](LICENSE). Individual project links remain the property of their respective authors.
