# 🛠️ Open-Source Fairness & Bias-Auditing Projects

Curated for the *AI Ethics & Social Justice Podcast* — live, active repositories where real debates about fairness measurement are happening right now. Compiled from hands-on GitHub research for our technology-and-social-justice episode.

---

## 1. [AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360) — Trusted AI / IBM

| | |
|---|---|
| ⭐ Stars | 2,863 |
| 🍴 Forks | ~770 |
| 👥 Contributors | IBM Research + open community (Trusted-AI org) |
| 📝 License | Apache-2.0 |
| 🌐 Docs | [aif360.mybluemix.net](https://aif360.mybluemix.net) |
| 🕒 Last updated | 2026-09-07 |

**What it does:** The most comprehensive, long-running bias-detection and mitigation toolkit in the ecosystem. Ships **70+ fairness metrics** for datasets and models plus **15+ debiasing algorithms** spanning preprocessing, in-processing, and postprocessing. Comes with reference datasets and executable Jupyter notebooks.

**Why it matters:** AIF360 is the "full menu" of fairness metrics — the place a practitioner goes to compute any mainstream group-fairness statistic and to trial a mitigation pass. Its scale makes it the default reference point whenever people argue about *which metric to use* and *when a metric is even appropriate*.

**Podcast angle:** With 70+ metrics, AIF360 embodies the uncomfortable fact that there is no single "fairness number." It is the ideal launchpad for an episode on why measuring fairness is itself a political and mathematical choice, not a button press.

---

## 2. [Fairlearn](https://github.com/fairlearn/fairlearn) — Microsoft

| | |
|---|---|
| ⭐ Stars | 2,286 |
| 🍴 Forks | ~200 |
| 👥 Contributors | Microsoft + academic maintainers (Tamara Atanasoska, Roman Lutz, others) |
| 📝 License | MIT |
| 📖 Docs | [fairlearn.org](https://fairlearn.org) |
| 🕒 Last updated | 2026-09-09 |

**What it does:** A widely-cited Python library for fairness assessment and mitigation. Implements demographic parity, equalized odds, counterfactual fairness, and constraint-based reduction methods (ExponentiatedGradient, ThresholdOptimizer), integrated directly with scikit-learn, PyTorch, and pandas pipelines.

**Why it matters:** Fairlearn is the industry's reference implementation and, crucially, it *frames fairness as a sociotechnical challenge*, not just an optimization problem. Its active issue tracker is where the community works through hard questions about scope, metrics, and who these tools should serve.

**Podcast angle (featured debate):** Fairlearn's maintainers state the project does *not* define a fixed list of sensitive features — users supply them. See [DEBATES.md → "Should a Fairness Tool Recognize 'Species' as a Sensitive Feature?"](../blob/main/DEBATES.md). This design choice — domain-general, any column can be sensitive — is itself a philosophical statement and the source of a live disagreement.

---

## 3. [Aequitas](https://github.com/dssg/aequitas) — Data Science for Good (UChicago)

| | |
|---|---|
| ⭐ Stars | 773 |
| 🍴 Forks | ~150 |
| 👥 Contributors | University of Chicago DSG team + open community |
| 📝 License | AGPL-3.0 |
| 🌐 Docs | [aequitas.dssg.io](https://aequitas.dssg.io) |
| 🕒 Last updated | 2026-09-07 |

**What it does:** A bias-auditing and fair-ML toolkit built on confusion-matrix-based fairness auditing. Computes key demographic-parity and equalized-odds differences across demographic groups, with an interactive API and a well-known "disparate impact remover" for preprocessing.

**Why it matters:** Aequitas was built for exactly the use-case this podcast cares about — auditing real, high-stakes models (criminal-justice risk scores, predictive policing, resource allocation) and producing accessible disparity reports. It bridges technical metrics and narrative, evidence-driven advocacy.

**Podcast angle:** Aequitas grew out of a project on algorithmic equity in the criminal-justice system, so it carries the policy stakes on its sleeve. It's a strong example of a tool built *with* affected communities in mind, not merely *about* them.

---

## 🔗 Exploring the Tension Between These Tools

These three repos don't just sit side by side — they disagree in productive ways:

- **Metric scope:** AIF360 stockpiles 70+ metrics; Fairlearn curates a smaller set and insists metrics are context-dependent; Aequitas centers a confusion-matrix lens tied to specific harms. *Which metric wins?* is a live debate.
- **Tool scope / who to serve:** Fairlearn stays deliberately domain-general (any column can be a sensitive feature) and recently had to decide whether to add non-traditional categories like "species"; Aequitas focuses on demographic groups in high-stakes US policy domains; AIF360 tries to be the umbrella. The boundary question — *where does one tool's responsibility end and another's begin?* — shapes who gets audited and by whom.
- **Open-source governance:** All three are institutional (IBM, Microsoft, UChicago) yet community-governed, raising the same questions the podcast tracks: whose expertise is centered, and whose communities are rendered visible (or invisible) by a tool's design choices.

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

Built for the AI Ethics & Social Justice Podcast. Listening, learning, and acting together.
