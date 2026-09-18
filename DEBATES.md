# ⚖️ Ongoing Debate: Is `average_odds_difference` a Valid Equalized Odds Metric?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)
**Opened:** April 23, 2024 by [@AndreFCruz](https://github.com/AndreFCruz)
**Status:** 🟢 Still open — 2 👍 reactions, 1 reply as of September 2026
**Podcast relevance:** ★★★★★ *This is the perfect case study for our episode.*

---

## The Claim

AndreFCruz, a developer and researcher, filed an issue reporting that the **`average_odds_difference`** metric in IBM's AI Fairness 360 toolkit is **mathematically incorrectly represented** as an "equalized odds" relaxation.

The [official documentation](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> *"A value of 0 indicates equality of odds."*

Andre argues this is **false**. He provides a geometric counterexample: there exist confusion matrices where `average_odds_difference = 0` but the model does **not** satisfy equalized odds. His screenshot shows two points on a line where the metric reports zero difference, yet the true TPR and FPR parity conditions required by equalized odds are violated.

The same error, he notes, appears in IBM's public-facing [fairness metrics documentation](https://dataplatform.cloud.ibm.com/docs/content/wsj/model/wos-fairness-metrics-ovr.html?context=cpdaas).

## Why This Matters

This is not a mere academic nitpick. The `average_odds_difference` metric is:

1. **Featured in the toolkit's default output** — practitioners running bias audits see this number and trust it as a measure of equalized odds compliance.
2. **Referenced in IBM's public documentation** — organizations making procurement or compliance decisions rely on these materials.
3. **A cornerstone of the "actionable" fairness narrative** — the claim that equalized odds can be achieved post-processing depends on this metric being correctly specified.

If the metric is wrong, then **organizations may believe they have achieved fairness when they have not** — and communities that depend on these audits may be falsely reassured.

## The Community Response

The issue received **2 👍 reactions**, signaling community agreement that this is a real problem. Then, in September 2026, [@Hanabi9248](https://github.com/Hanabi9248) responded:

> *"Could you assign this to me? The cancellation also appears in MetricTextExplainer and its JSON output. I have a focused correction for both API docstrings and the explanations, with a four-row example where average_odds_difference is 0 while average_abs_odds_difference and equalized_odds_difference are both 1. The metric formulas remain unchanged."*

This reply reveals several layers of the debate:

- **The bug is not just in documentation** — it propagates into the **MetricTextExplainer** (the natural-language explanation engine) and its **JSON output** (the machine-readable audit report). This means the error is systemic, not cosmetic.
- **A fix is possible but requires careful articulation** — Hanabi9248 proposes preserving the metric formulas while correcting the *explanations* and *docstrings*. This raises the question: *Is the metric itself mathematically wrong, or just its interpretation?*
- **The four-row counterexample** — a concrete, minimal demonstration where `average_odds_difference = 0` but both `average_abs_odds_difference = 1` and `equalized_odds_difference = 1` — makes the abstract argument tangible. This is the kind of evidence that could reshape how fairness metrics are taught.

## The Deeper Questions for Our Podcast

This single issue opens up several fundamental debates about algorithmic fairness:

### 1. 🔬 Who gets to define "fairness"?
AIF360 is maintained by IBM Research. Fairlearn is maintained by Microsoft. Both define fairness metrics that countless organizations adopt. When a metric is mathematically flawed, **who bears responsibility** — the maintainers, the institutions, or the field that standardized on the tool?

### 2. 📐 Can fairness be reduced to a number?
The issue reveals a tension between **mathematical precision** (the metric doesn't match its name) and **practical utility** (practitioners need actionable signals). If we can't even agree on what "average odds difference" *means*, how can we use it to make decisions about real people's lives?

### 3. 🤝 Who should a fairness tool serve?
Fairlearn's README explicitly states: *" fairness is fundamentally a sociotechnical challenge. Many aspects of fairness, such as justice and due process, are not captured by quantitative fairness metrics."* But AIF360's documentation treats fairness as a measurable quantity with clear thresholds. **Which philosophy better serves the communities most impacted by algorithmic decisions?**

### 4. 🔧 Is open-source enough to catch these errors?
The issue was open in April 2024 and still hasn been assigned a formal fix — only a community volunteer offered to help. **Does the open-source model adequately protect against harms caused by incorrect technical definitions in widely-deployed tools?**

---

## Key Takeaway for Listeners

> *When the tools we use to measure justice are themselves unjust, the result isn't just a bug — it's a betrayal of trust. The debate over `average_odds_difference` isn't about notation; it's about whether fairness can be automated at all, or whether it requires a conversation that no metric can replace.*

---

*To contribute your own analysis or follow-up, comment on the [original issue](https://github.com/Trusted-AI/AIF360/issues/528) or open a discussion in this repo.*