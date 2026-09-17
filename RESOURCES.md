# 📚 Open-Source Fairness & Bias Auditing Resources

A curated catalog of active open-source projects related to algorithmic fairness, bias auditing, and AI ethics — tracked for the *AI Ethics & Social Justice* podcast.

---

## 🏆 Featured Projects

### 1. [IBM AIF360 (AI Fairness 360)](https://github.com/Trusted-AI/AIF360) — *Comprehensive Fairness Metrics & Bias Mitigation*

| | |
|---|---|
| **Stars** | 2,866 ⭐ |
| **Language** | Python |
| **License** | Apache 2.0 |
| **Last Updated** | September 2026 |

**What it does:** A comprehensive toolkit from IBM Research providing a unified set of fairness metrics for datasets and machine learning models, explanations for these metrics, and algorithms to mitigate bias. Covers pre-processing (resampling, reweighing), in-processing (adversarial debiasing, threshold optimization), and post-processing (calibrated equalized odds, reject opportunity) methods.

**Why it matters for the podcast:** AIF360 is the most widely cited fairness toolkit in academic research and enterprise settings. Its metric library includes demographic parity, equalized odds, predictive parity, equal opportunity, and more — each with detailed documentation and worked examples. It's also the site of **Debate 4** in `DEBATES.md`: a long-running issue arguing that the `average_odds_difference` metric is wrongly documented as an equalized odds relaxation, raising the question of whether the documentation itself shapes what "fairness" means.

**Key feature:** The `MetricTextExplainer` class generates natural-language explanations of fairness metrics — bridging the gap between technical measurement and policy communication.

**Run it locally:**
```bash
pip install aif360
python examples/adult_example.py   # end-to-end audit on Adult Income dataset
```

**Podcast angle:** The metric documentation debate (see `DEBATES.md`) is a perfect case study: if the foundational toolkit gets its own metric definitions wrong, what does that mean for every downstream audit, policy brief, and courtroom presentation that cites those metrics?

---

### 2. [Aequitas](https://github.com/dssg/aequitas) — *Bias Auditing & Fair ML Toolkit*

| | |
|---|---|
| **Stars** | 773 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** A bias auditing toolkit developed at the University of Chicago's Data and Society Project. It enables developers to cross-examine ML models against key fairness definitions (demographic parity,充分条件, equalized odds) at both the global and individual-class level. Designed for integration into existing ML pipelines rather than as a standalone audit framework.

**Why it matters for the podcast:** Aequitas takes a deliberately pragmatic approach — it's built for developers who need to audit models in production, not researchers exploring fairness theory. Its emphasis on per-class fairness analysis (not just aggregate metrics) surfaces the intersectional problem: a model can satisfy demographic parity globally while severely violating it for specific subgroups.

**Key feature:** The "Group Equity" report card system thatScore each protected group on multiple fairness criteria simultaneously, making trade-offs visible rather than hidden behind a single aggregate number.

**Podcast angle:** The tension between Aequitas's pragmatic developer-focus and the deeper theoretical questions about fairness metric selection is itself an episode topic: can you build a tool that's useful without taking a stance on which fairness definition is morally correct?

---

### 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit) — *Enterprise-Grade AI Assurance*

| | |
|---|---|
| **Stars** | 310 ⭐ |
| **Language** | Python |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** A comprehensive responsible-AI platform covering fairness, bias, safety, security, privacy, explainability, and hallucination detection for both traditional ML and LLMs. The fairness module implements statistical parity difference, disparate impact ratio, four-fifths rule, and Cohen's D for detection, plus equalized odds and re-weighing for mitigation. Includes a micro-frontend dashboard for experimentation and reporting.

**Why it matters for the podcast:** This toolkit represents the "enterprise compliance" approach to AI ethics — broad in scope, polished in presentation, and designed for organizational governance rather than research exploration. The contrast with AIF360 (research-grade, metric-deep) and Aequitas (pragmatic, per-class) reveals three different philosophies about who fairness tools are for and what they should prioritize.

**Key feature:** The modular microservice architecture (separate services for fairness, safety, privacy, etc.) allows organizations to adopt only the components they need — a stark contrast to the monolithic design of academic toolkits.

**Podcast angle:** The enterprise vs. academic tension: should fairness tools be built by and for technologists in companies, or be open research artifacts accessible to civil society and affected communities?

---

### 4. [Fair-Code](https://github.com/yakew7/Fair-Code) — *Algorithmic Bias Detection & Mitigation*

| | |
|---|---|
| **Stars** | 47 ⭐ |
| **Forks** | 45 |
| **Language** | Python, HTML, Jupyter Notebook |
| **License** | MIT |
| **Last Updated** | September 2026 |

**What it does:** Seven complete bias audits across criminal justice (COMPAS), hiring, lending, insurance denial, welfare eligibility, healthcare readmission, and tenant screening. Each audit ships as a reproducible `unfair.py` / `fair.py` pair plus Jupyter notebooks walking through the full pipeline: train a biased model → measure the fairness gap → identify proxy variables → engineer a fair model → measure again.

**Why it matters for the podcast:** Fair-Code is the most essayistic project in the fairness space — its 61 explainers cover everything from "What Is a Proxy Variable?" to "Why Fairness Metrics Conflict." It's also the site of the **featured debates** in `DEBATES.md` (Debates 1–3).

**Key feature:** The [Open Dataset Profiler](https://github.com/yakew7/Fair-Code#open-dataset-profiler) audits datasets themselves for demographic representation — a different lens from model-level fairness.

**Run it locally:**
```bash
pip install -r requirements.txt
python COMPAS/unfair.py   # see the bias
python COMPAS/fair.py     # see the fix
```

**Podcast angle:** The ProPublica vs. Northpointe COMPAS dispute is the showpiece — two parties both mathematically correct because they measured different fairness criteria. Episode material writes itself.

---

### 5. [Algorithmic-Fairness-Toolkit](https://github.com/NikhilRajBharti26-gif/Algorithmic-Fairness-Toolkit) — *Audit & Mitigation Comparison*

| | |
|---|---|
| **Language** | Python |
| **Key dependencies** | Fairlearn, scikit-learn, Pandas, Streamlit |
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

## 🗺️ The Fairness Landscape — Quick Reference

| Project | Focus | Maturity | Best For |
|---|---|---|---|
| **IBM AIF360** | Comprehensive metric library + mitigation algorithms | Production-grade, widely cited in research | Deep metric comparison, enterprise-grade bias mitigation |
| **Aequitas** | Per-group fairness auditing for production pipelines | Pragmatic, developer-focused | Integrating audits into existing ML workflows |
| **Infosys RAI** | Enterprise governance + broad AI assurance (fairness, safety, privacy) | Enterprise-grade, modular architecture | Organizational compliance, regulatory mapping |
| **Fair-Code** | Research audits + plain-language explainers | Production-quality audits, active development | Deep dives on specific domains (healthcare, justice, lending) |
| **Algorithmic-Fairness-Toolkit** | Metric comparison + trade-off analysis | Functional toolkit with Streamlit dashboard | Understanding what you lose when you gain fairness |

---

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
- [IBM AIF360 Documentation](https://aif360.readthedocs.io/) — Metric definitions and usage guides
- [Aequitas Documentation](https://dssg.github.io/aequitas/) — Bias auditing guides and API reference

---

*Contributors: Add your favourite fairness project to `RESOURCES.md` — just follow the table format above and include stars, license, and a one-line pitch.*