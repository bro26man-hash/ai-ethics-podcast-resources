# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of active open-source projects related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects

### 1. [Fair-Code](https://github.com/yakew7/Fair-Code) — *Algorithmic Bias Detection & Mitigation*

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Forks** | 45 |
| **Language** | Python, HTML, Jupyter Notebook |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** Seven complete bias audits across criminal justice (COMPAS), hiring, lending, insurance denial, welfare eligibility, healthcare readmission, and tenant screening. Each audit ships as a reproducible `unfair.py` / `fair.py` pair plus Jupyter notebooks walking through the full pipeline: train a biased model → measure the fairness gap → identify proxy variables → engineer a fair model → measure again.

**Why it matters for the podcast:** Fair-Code is the most essayistic project in the fairness space — its [61 explainers](https://github.com/yakew7/Fair-Code/tree/main/explainers) cover everything from "What Is a Proxy Variable?" to "Why Fairness Metrics Conflict." It's also the site of the **featured debates** in `DEBATES.md`.

**Key feature:** The [Open Dataset Profiler](https://github.com/yakew7/Fair-Code#open-dataset-profiler) audits datasets themselves for demographic representation — a different lens from model-level fairness.

**Run it locally:**
```bash
pip install -r requirements.txt
python COMPAS/unfair.py   # see the bias
python COMPAS/fair.py     # see the fix
```

**Podcast angle:** The ProPublica vs. Northpointe COMPAS dispute is the showpiece — two parties both mathematically correct because they measured different fairness criteria. Episode material writes itself.

---

### 2. [Algorithmic-Fairness-Toolkit](https://github.com/NikhilRajBharti26-gif/Algorithmic-Fairness-Toolkit) — *Audit & Mitigation Comparison*

| | |
|---|---|
| **Language** | Python |
| **Key依赖** | Fairlearn, scikit-learn, Pandas, Streamlit |
| **License** | — |
| **Last Updated** | June 2026 |

**What it does:** A structured toolkit that audits three real datasets (Adult Income, German Credit, COMPAS) against four fairness metrics (demographic parity, equalized odds, equal opportunity, predictive parity) and then applies three genuinely different mitigation techniques — pre-processing (Reweighing), in-processing (Fairlearn's ExponentiatedGradient), and post-processing (ThresholdOptimizer) — comparing their real accuracy/fairness trade-offs.

**Why it matters for the podcast:** The toolkit's own README states the key insight: *"all three techniques successfully reduce the demographic parity gap at the cost of some accuracy — but they do not uniformly improve every fairness metric. Aggressively closing the demographic parity gap here actually widens the equalized-odds and predictive-parity gaps."* This is the impossibility theorem made concrete.

**Run it locally:**
```bash
streamlit run app/streamlit_app.py   # interactive dashboard
python scripts/run_adult_experiment.py # reproducible CLI experiment
```

**Podcast angle:** The trade-off analysis is perfect for an episode segment on "what do you lose when you gain fairness?" — the Pareto-front analysis makes the cost literal.

---

### 3. [FairMind](https://github.com/adhit-r/fairmind) — *AI Assurance Control Plane*

| | |
|---|---|
| **Stars** | 9 ⭐ |
| **Forks** | 12 |
| **Language** | Python (FastAPI), TypeScript (Next.js) |
| **License** | MIT |
| **Last Updated** | August 2026 |

**What it does:** An evolving governance platform for evidence-grade AI evaluation — bias detection for predictive models, LLMs, and multimodal systems; remediation code generation; regulatory compliance mapping (EU AI Act, GDPR, NIST AI RMF); and model registry with bias history tracking. Currently in an internal trust-foundation alpha, building the evidence contract infrastructure before shipping evaluation engines.

**Why it matters for the podcast:** FairMind represents the "compliance-first" approach — the question isn't just "is this model fair?" but "can you prove it, reproducibly, with an audit trail?" This is the enterprise/governance perspective that contrasts with the research-oriented Fair-Code toolkit.

**Podcast angle:** The tension between Fair-Code (research-first, transparent, but not auditable) and FairMind (audit-first, evidence-grade, but not yet functional) is itself a debate: should fairness tools be open research a her: "should fairness tools be open research artifacts or governed certifying platforms?"

---

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Focus | Maturity | Best For |
|---|---|---|---|
| **Fair-Code** | Research audits + explainers | Production-quality audits, active development | Deep dives on specific domains (healthcare, justice, lending) |
| **Algorithmic-Fairness-Toolkit** | Metric comparison + trade-off analysis | Functional toolkit with Streamlit dashboard | Understanding what you lose when you gain fairness |
| **FairMind** | Governance & compliance assurance | Internal alpha, building evidence infrastructure | Enterprise audit trails, regulatory mapping |

## 📖 Essential Explainers (from Fair-Code)

These 61 plain-language explainers are among the best free educational resources on algorithmic fairness:

**Core concepts:**
- [What Is a Proxy Variable?](https://github.com/yakew7/Fair-Code/blob/main/explainers/proxy-variables.md) — Why removing race isn't enough
- [Why Fairness Metrics Conflict](https://github.com/yakew7/Fair-Code/blob/main/explainers/fairness-metric-conflicts.md) — The mathematical impossibility theorem
- [What Is Counterfactual Fairness?](https://github.com/yakew7/Fair-Code/blob/main/explainers/counterfactual-fairness.md) — The causal approach to fairness
- [What Is Demographic Parity?](https://github.com/yakew7/Fair-Code/blob/main/explainers/demographic-parity.md) — The equal-outcome-rate metric
- [What Is Equalized Odds?](https://github.com/yakew7/Fair-Code/blob/main/explainers/equalized-odds.md) — The equal-error-rate metric
- [What Is Predictive Parity?](https://github.com/yakew7/Fair-Code/blob/main/explainers/predictive-parity.md) — The equal-reliability metric

**Healthcare focus:**
- [Why Accuracy Is Not Enough in Healthcare AI](https://github.com/yakew7/Fair-Code/blob/main/explainers/accuracy-not-enough-healthcare-ai.md)
- [Race Correction in Clinical Algorithms](https://github.com/yakew7/Fair-Code/blob/main/explainers/race-correction-clinical-algorithms.md)
- [The Obermeyer Case: When Cost Becomes a Proxy for Health Need](https://github.com/yakew7/Fair-Code/blob/main/explainers/obermeyer-cost-proxy.md)

**Advanced topics:**
- [What Is Intersectional Bias?](https://github.com/yakew7/Fair-Code/blob/main/explainers/intersectional-bias.md)
- [What Is Differential Privacy (and Its Tension With Fairness)?](https://github.com/yakew7/Fair-Code/blob/main/explainers/differential-privacy.md)
- [What Is Simpson's Paradox in Fairness Audits?](https://github.com/yakew7/Fair-Code/blob/main/explainers/simpsons-paradox.md)

## 🔗 External Resources

- [Fair-Code live site](https://www.thefaircode.xyz) — Hosted explainers and interactive profiler
- [ProPublica: Machine Bias (2016)](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) — The investigation that started the COMPAS debate
- [Chouldechova (2017): Fair Prediction with Disparate Impact](https://arxiv.org/abs/1703.00056) — The impossibility theorem proof
- [Kleinberg, Mullainathan & Raghavan (2016): Inherent Trade-offs](https://arxiv.org/abs/1609.05807) — Independent impossibility result
- [Barocas & Hardt: Fairness and Machine Learning](https://fairmlbook.org/) — Free textbook, Chapters 2 and 4 directly relevant
- [Kusner et al. (2017): Counterfactual Fairness](https://arxiv.org/abs/1703.06856) — NeurIPS paper introducing the causal framework

---

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*
