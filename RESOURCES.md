# 🛠️ Open-Source Fairness & Bias-Auditing Projects

Curated for the *AI Ethics & Social Justice Podcast* — live, active repositories where real debates about fairness measurement are happening right now.

---

## 1. [AIF360 (Trusted-AI)](https://github.com/Trusted-AI/AIF360) — IBM Research / Trusted-AI

| | |
|---|---|
| ⭐ Stars | 2,863 |
| 🍴 Forks | 910 |
| 👥 Contributors | Dozens of research contributors; maintainers across IBM, academia |
| 📝 License | Apache-2.0 |
| 🌐 Website | [aif360.res.ibm.com](https://aif360.res.ibm.com) |
| 🐍 Stack | Python (also R) |

**What it does:** The most comprehensive set of fairness metrics (70+) and debiasing algorithms (15+) for datasets and ML models. Covers pre-processing (Reweighing, Disparate Impact Remover, Optimized Preprocessing), in-processing (Adversarial Debiasing, Meta-Algorithm for Fair Classification, Learning Fair Representations), and post-processing (Equalized Odds Postprocessing, Calibrated Equalized Odds). Includes metrics for group fairness, sample distortion, generalized entropy, bias amplification, and rich subgroup fairness.

**Why it matters:** AIF360 is the reference implementation against which many fairness debates are framed. Its sheer breadth — every major fairness notion operationalized — is exactly where definitional confusion lives: the toolkit gives you every fairness definition you can think of, without telling you which one is *right* for the community you claim to serve. It is the scene of both the measurement debates (see [DEBATES.md](#debatesmd)) and the mitigation-forgiveness questions ("does fixing the data paper over the real harm?").

**Podcast angle:** The docs warn that "it may be confusing to figure out which metrics and algorithms are most appropriate for a given use case." That warning *is* the episode.

---

## 2. [Fairlearn](https://github.com/fairlearn/fairlearn) — Microsoft

| | |
|---|---|
| ⭐ Stars | 2,284 |
| 🍴 Forks | ~200 |
| 👥 Contributors | Core team: Microsoft + academic maintainers (Tamara Atanasoska, Roman Lutz, others) |
| 📝 License | MIT |
| 📖 Docs | [fairlearn.org](https://fairlearn.org) |

**What it does:** The most widely-cited Python library for fairness assessment and mitigation. Implements demographic parity, equalized odds, counterfactual fairness, and constraint-based reduction methods (ExponentiatedGradient, ThresholdOptimizer). Integrates directly with scikit-learn, PyTorch, and pandas pipelines.

**Why it matters:** Fairlearn is the industry's reference implementation. When a debate breaks out inside Fairlearn's issue tracker, it reflects genuine disagreement among the people building the tools that millions of developers will eventually use. The current open discussion about scope, metrics, and who these tools should serve (see [DEBATES.md](#debatesmd)) illustrates the tension between a tool's stated mission and its practical boundaries.

**Podcast angle:** Fairlearn's maintainers explicitly say the project does *not* define a fixed list of sensitive features — users supply them. This design choice is itself a philosophical statement: fairness auditing is a context-dependent practice, not a plug-and-play checklist. That tension is the perfect hook for a discussion with listeners.

---

## 3. [Fair-Code](https://github.com/yakew7/Fair-Code) — yakew7

| | |
|---|---|
| ⭐ Stars | 48 (growing fast in 2026) |
| 🍴 Forks | 27 |
| 👥 Contributors | 23 |
| 📝 License | MIT |
| 🌐 Website | [thefaircode.xyz](https://www.thefaircode.xyz) |

**What it does:** End-to-end algorithmic bias detection and mitigation framework. Seven domain-specific audits — Criminal Justice (COMPAS), Hiring, Lending, Insurance Denial, Welfare Eligibility, Healthcare Readmission, and Tenant Screening — each with Jupyter notebooks, contaminated-vs-mitigated model comparisons, and a bias-fairness-reduction percentage.

**Why it matters:** Fair-Code is unusually concrete. It doesn't just theorize about fairness metrics — it selects a specific dataset, trains a biased model, drops protected attributes and proxy variables, retrains, and shows the exact gap reduction. Its companion tool, the Open Dataset Profiler, audits raw datasets for demographic under-representation. A benchmark harness (`faircode bench`) applies five mitigation strategies (S0–S4) across six fairness metrics and multiple model families. It also includes 53 plain-language explainers covering everything from false positive/negative asymmetry in healthcare to proxy entanglement and reject inference.

**Podcast angle:** Fair-Code's Healthcare Readmission audit (#06) produced a counter-intuitive result: mitigations *increased* the gender gap from 0.02% → 0.04% without careful proxy handling. That result alone could fuel an entire episode about why imperfect mitigations can make things worse for some groups even as they improve others.

---

## 4. [CognitiveLens](https://github.com/AmirhosseinHonardoust/Cognitivelens-AI-Human-Comparison) — AmirhosseinHonardoust

| | |
|---|---|
| ⭐ Stars | 20 |
| 🍴 Forks | 0 |
| 👥 Contributors | 1 (original author) |
| 📝 License | MIT |
| 🛠️ Stack | Python, Streamlit, Plotly |

**What it does:** An interactive Streamlit dashboard for comparing human vs. AI decision-making. Computes Cohen's κ, AUC, Brier score, and subgroup fairness gaps. Users upload their own CSV data and explore where models diverge from human judgments across demographics.

**Why it matters:** CognitiveLens foregrounds the *alignment* problem — not just "is this biased?" but "does the AI agree with human judgment, and if so, is that agreement itself biased?" The tool's calibration views make it easy to spot cases where models are confidently wrong for specific subgroups, which is where fairness harms are most acute.

**Podcast angle:** What happens when human labels are themselves biased? CognitiveLens lets you overlay human and AI decisions, making that question visible. A clinician who agrees with an AI's discriminatory recommendation appears as "alignment," not injustice — and the tool makes that paradox clear.

---

## Honorable Mentions

### [ai-bias-audit-tool](https://github.com/srhill12/ai-bias-audit-tool) — srhill12
A Streamlit app for auditing algorithmic fairness in AI systems using the COMPAS recidivism dataset, aligned with the **NIST AI RMF**. A practitioner-facing audit workflow that makes a high-stakes criminal-justice model's disparity visible — and frames the fair choice in ready-for-regulatory terms.

### [LiFT](https://github.com/linkedin/LiFT) (LinkedIn) — 173 ⭐
Scala/Spark toolkit for measuring fairness at web scale with permutation testing.

### [Aequitas](https://github.com/dssg/aequitas) (UChicago) — 772 ⭐
Confusion-matrix-based fairness auditing with an interactive API, widely used in policy contexts.

### [Responsibly](https://github.com/ResponsiblyAI/responsibly) — 100 ⭐
Python-first auditing toolkit aligned with *Fairness and Machine Learning* (Barocas, Hardt & Narayanan); includes word-embedded bias metrics.

### Unbiased AI Decision — Saikrishna-dev-oss
A smaller, prototype web tool for running demographic-parity audits against CSV uploads. A contrast case: a one-person project whose simplicity raises its own questions about depth vs. accessibility in fairness tooling.

---

## How to Add a Project

1. Fork this repo
2. Create a new section in `RESOURCES.md` following the table format above
3. Include: name, owner, GitHub link, key stats (stars, forks, license), what it does, why it matters, and a podcast angle
4. Open a PR with your additions

We prioritize tools that:
- Have been updated within the last 12 months
- Have a clear open-source license
- Center communities most affected by algorithmic harm, not just technologists
- Surface genuine debates about fairness measurement or tool scope
