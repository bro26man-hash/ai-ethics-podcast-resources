# 📚 Open-Source Fairness & Bias-Auditing Projects

A curated collection of active, well-maintained open-source projects working on algorithmic fairness and bias auditing — sourced from real GitHub repositories.

---

## 🏆 Featured Projects

### 1. [AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)
- **Maintainer**: IBM / Trusted-AI
- **Stars**: ⭐ 2,862 | **Language**: Python
- **Last updated**: September 2026 (actively maintained)
- **License**: Apache 2.0

**What it does**: A comprehensive toolkit with 70+ fairness metrics and 15+ bias-mitigation algorithms. Covers pre-processing, in-processing, and post-processing methods. Includes interpretability modules and a `MetricTextExplainer` for natural-language explanations of fairness audit results.

**Why it matters for the podcast**: AIF360 is one of the most widely cited fairness toolkits in academia and industry. Its scale — 70+ metrics — raises the exact questions our episode explores: *Can we really have a single number that captures "fairness"? When a metric says 0, does that actually mean fair?*

**Notable ongoing debate**: [Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — A contributor argues that the `average_odds_difference` metric is mathematically misrepresented in its documentation, calling into question whether the metric's name matches what it actually measures.

---

### 2. [Aequitas](https://github.com/dssg/aequitas)
- **Maintainer**: University of Chicago (Data & Society Project Group)
- **Stars**: ⭐ 773 | **Language**: Python  
- **Last updated**: September 2026 (actively maintained)
- **License**: MIT

**What it does**: A bias-auditing toolkit designed for data scientists, ML researchers, and policymakers. Provides confusion-matrix-based fairness metrics with an interactive API and visualization tools. Version 1.0 ("Aequitas Flow") adds full Fair ML experimentation — pre-processing, in-processing, and post-processing methods.

**Why it matters for the podcast**: Aequitas explicitly targets *non-technical* audiences (policymakers, auditors) alongside engineers. Its design philosophy — making fairness auditing accessible to people outside ML — is itself a sociotechnical choice worth examining. Who gets to decide what "fair" looks like?

**Key feature**: Built-in datasets (BankAccountFraud, FolkTables) and Colab notebooks that walk users through real bias audits with the COMPAS recidivism dataset.

---

### 3. [Responsibly](https://github.com/ResponsiblyAI/responsibly)
- **Maintainer**: ResponsiblyAI
- **Stars**: ⭐ 101 | **Language**: Python
- **Last updated**: September 2026 (actively maintained)
- **License**: MIT

**What it does**: A Python-first auditing toolkit aligned with the textbook *Fairness and Machine Learning* (Barocas, Hardt & Narayanan). Three sub-packages: dataset benchmarks, binary-classification fairness metrics & interventions, and word-embedding bias measurement.

**Why it matters for the podcast**: Responsibly's explicit alignment with a canonical academic text raises an interesting question: *Should fairness tools be dictated by a single theoretical framework?* The book's authors (Barocas, Hardt, Narayanan) are hugely influential — but their framing of fairness as primarily a classification problem has also been critiqued for narrowing what "fairness" can mean.

**Special focus**: Includes NLP-specific bias metrics (word embeddings), making it one of the few toolkits that bridges traditional ML fairness and NLP fairness.

---

## 📊 Quick Comparison

| Feature | AIF360 | Aequitas | Responsibly |
|---------|--------|----------|------------|
| Stars | 2,862 | 773 | 101 |
| Metrics | 70+ | ~15 confusion-matrix-based | Standards from Barocas et al. |
| Mitigation methods | 15+ | Pre/in/post-processing | Pre/in/post-processing |
| NLP support | Limited | No | Yes (word embeddings) |
| Target audience | Practitioners & researchers | Practitioners, policymakers, researchers | Practitioners & researchers |
| Active maintenance | ✅ Updated Sep 2026 | ✅ Updated Sep 2026 | ✅ Updated Sep 2026 |
| License | Apache 2.0 | MIT | MIT |

---

## 🔍 How to Use This Resource

- **Podcast listeners**: Start with the project whose audience matches yours. Aequitas if you're new to fairness auditing. AIF360 if you want the full technical picture. Responsibly if you care about NLP.
- **Contributors**: Add your own favorite fairness tools below. Follow the format above.
- **Debaters**: Check `DEBATES.md` for real GitHub controversies pulled from these projects.

---

## 📖 Further Reading

- [Fairness and Machine Learning (Barocas, Hardt & Narayanan)](https://fairmlbook.org) — the canonical textbook that shapes most fairness tooling
- [Aequitas tutorial (KDD/AAAI)](https://github.com/dssg/fairness_tutorial) — deep dive into fairness auditing methodology
- [IBM Fairness Metrics documentation](https://aif360.readthedocs.io/) — reference for all AIF360 metrics

---

*This is a living document. To suggest additions or corrections, [open an issue](https://github.com/bro26man-hash/ai-ethics-podcast-resources/issues/new).*
