# ⚖️ Live Debates from Open-Source Fairness Repos

Real controversies happening in real repos. Each entry is based on an actual open GitHub issue thread, verified as of September 2026.

---

## 🔴 Debate #1: The Average-Odds Documentation Bug — Who Owns the Definition of "Zero"?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Project:** [IBM AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Opened:** April 23, 2024  |  **Last updated:** September 11, 2026  |  **Status:** Open  |  **👍 Reactions:** 2  |  **💬 Comments:** 1  
**Labels:** None (no maintainer has categorized or assigned it)  
**Issue author:** [AndreFCruz](https://github.com/AndreFCruz)  
**Volunteer responder:** [Hanabi9248](https://github.com/Hanabi9248) (September 11, 2026)

### The Claim

**AndreFCruz**, a developer and external contributor, filed an issue arguing that the `average_odds_difference` metric in AIF360 is **mathematically misdocumented**. The [API docstring](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> *"A value of 0 indicates equality of odds."*

Andre says this is **false**. He provides a visualization (attached to the issue) showing that there exist pairs of ROC curves where `average_odds_difference = 0` but **equalized odds do not hold**. The error, he points out, is not just in the AIF360 documentation — it also appears on **IBM's own fairness metrics webpage**.

### Why This Matters

The "average odds difference" metric is not an obscure corner case. It is one of the standard metrics shipped in the most widely used fairness toolkit in the world. If the documentation tells practitioners that zero means equality of odds — and that is not actually true — then:

1. **Audits can pass when they shouldn't.** A model could score 0 on average odds difference and still violate equalized odds, leading to a false sense of fairness.
2. **The error propagates.** IBM's website, tutorial notebooks, and downstream documentation all repeat the claim. Anyone learning fairness from AIF360 inherits the mistake.
3. **The definition of fairness is being silently distorted.** When a toolkit's docs are wrong about what a metric means, the toolkit shapes how an entire field thinks about fairness.

### The Response — Or Lack Thereof

After **17 months** with no maintainer response, **Hanabi9248** (a volunteer) commented on September 11, 2026:

> *"Could you assign this to me? The cancellation also appears in MetricTextExplainer and its JSON output. I have a focused correction for both API docstrings and the explanations, with a four-row example where average_odds_difference is 0 while average_abs_odds_difference and equalized_odds_difference are both 1. The metric formulas remain unchanged."*

This is a stunning admission in its own right: the error is **not just in the docs** — it's also in the `MetricTextExplainer`, the tool that generates human-readable explanations of fairness results. If the explainer says "this model achieves equalized odds" when it doesn't, the mismatch is not academic. It's operational.

### The Deeper Question: Who Decides What Zero Means?

This issue is really about **epistemic authority** in open-source fairness tools:

- **Is a metric what its formula says it is?** The formulas aren't changing — Hanabi9248 confirmed that. The math is fine. The problem is the *interpretation* attached to the result.
- **Does the documentation create reality?** If 1,000 practitioners read "A value of 0 indicates equality of odds" and trust it, does that make it true in practice — even if it's mathematically wrong?
- **Who bears the cost of a doc bug?** AndreFCruz filed it. Hanabi9248 offered to fix it. But no AIF360 maintainer has merged the correction in 17 months. The people closest to the fix are not the people with commit access.
- **Should fairness tools be more humble?** Maybe the real issue is that any single scalar claiming to capture "equality of odds" deserves a warning label — not a docstring that pretends it's definitive.

### Key Quotes from the Thread

> **AndreFCruz** (issue author): *"This mistake seems to be repeated on the IBM website"*  
> — Implying the error is systemic, not just a GitHub oversight.

> **Hanabi9248** (volunteer, Sept 2026): *"The metric formulas remain unchanged."*  
> — Clarifying that this is a documentation fix, not a mathematical one. The tool works fine. The words are wrong.

### The Broader AIF360 Maintenance Picture

This documentation bug is not an isolated case. During research, we found several other open issues in the AIF360 repo that paint a picture of a project under strain:

- **[#548 — Website is down](https://github.com/Trusted-AI/AIF360/issues/548):** Open since February 2025 with 7 comments. The official documentation website has been unavailable, making it harder for users to access metric definitions. A dead website + a misleading docstring = a double documentation failure.
- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric for intersectional analysis, but the existing metrics still have documentation errors. This raises the question: should you add new metrics before fixing the old ones?
- **[#526 — Memory Management Issue in ClassificationMetric](https://github.com/Trusted-AI/AIF360/issues/526):** Open since April 2024. A core class has performance problems.
- **[#379 — Add ThresholdOptimizer to sklearn-compatible post-processing](https://github.com/Trusted-AI/AIF360/issues/379):** Open since September 2022. A basic usability enhancement, labeled "good first issue."

The pattern: **documentation errors, infrastructure decay, and performance issues** all coexist. Volunteer corrections are welcome but unassigned.

### Discussion Prompts for the Episode

1. Should fairness toolkits be required to publish formal verification of their metric definitions — the way cryptographic libraries publish formal proofs?
2. Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
3. If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
4. Is 17+ months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?
5. When two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?
6. Is a volunteer-submitted fix with no maintainer merge pathway actually authoritative?
7. Should fairness tool maintainers be required to respond to issues within a certain timeframe? What would that look like?
8. If the documentation is wrong but the code is right, should you trust the tool? What if the documentation is right but the code is wrong?

---

## 🔴 Debate #2: The Intersectional Analysis Gap — Should Fairness Tools Return a Scalar or a Breakdown?

**Source:** [Trusted-AI/AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558)  
**Project:** [IBM AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Opened:** January 21, 2026  |  **Last updated:** September 16, 2026  |  **Status:** Open  
**Issue author:** [jetverbeek](https://github.com/jetverbeek)  
**Volunteer responder:** [Hanabi9248](https://github.com/Hanabi9248) (September 16, 2026)

### The Question

The current implementation of the Empirical Differential Fairness (EDF) metric only returns a **single scalar value** summarizing fairness across protected attributes. But for intersectional analysis, a single number hides the story: which specific combinations of attribute groups contribute most to the maximum log-ratio?

jetverbeek asked for the metric to return:
- The pair(s) of attribute groups that produce the max log ratio
- A breakdown of all/top X group combinations and their respective log-ratios

### The Volunteer's Proposal

After two months, **Hanabi9248** responded on September 16, 2026:

> *"I would keep smoothed_empirical_differential_fairness() returning a scalar and add a separate method for the group-pair breakdown, including the pairs attaining the maximum and an optional top-k limit. The calculations would use the same instance weights and Dirichlet smoothing as the existing metric, with tests for ties and multiple protected attributes. Please let me know if you prefer an optional details argument on the existing method instead."*

Key observations:
- Hanabi9248 proposed a **backward-compatible** design — the scalar return stays, a new method adds the breakdown
- The proposal preserves the existing mathematical framework (Dirichlet smoothing, instance weights)
- It includes **tests for edge cases** (ties, multiple protected attributes)
- Yet again, the issue remains **unassigned and unmerged**

### The Two Sides

**Side A — Single scalar is enough for most use cases.** Regulatory frameworks (EU AI Act, FTC guidance) typically ask for a single fairness score per protected attribute. A scalar is simpler to report, simpler to compare, and simpler to regulate against.

**Side B — Intersectional analysis is essential for real fairness.** A single scalar can hide severe disparities for specific subpopulations. Black women, disabled immigrants, Indigenous transgender people — these groups exist at the intersection of multiple protected attributes, and a scalar that averages across all of them can erase their specific experience of discrimination.

### Why It Matters

1. **The scalar vs. breakdown tension is a political choice.** A single "fairness score" simplifies regulation but can erase the communities that need the most protection.
2. **Who decides the granularity of fairness reporting?** Regulators, practitioners, or the communities being audited? Currently, the answer is: whoever maintains the toolkit.
3. **The volunteer-maintenance pattern, again.** Both AIF360 debates (#528 and #558) share the same pattern: a volunteer offers a fix or enhancement, but the issue sits unassigned. Is IBM Research delegating fairness standardization to volunteers? And should we trust them to do so?
4. **The "360" in AIF360.** If the toolkit can't tell you which specific group combinations are most disadvantaged, is it really measuring fairness along 360 degrees? Or is it only measuring fairness along single axes?

### Discussion Prompts

1. Should fairness tools be required to support intersectional analysis by default, or is a single scalar acceptable for regulatory contexts?
2. If a single scalar can mask the worst-off group, is it ethical to ship that as the default output?
3. Who should pay for the maintenance of fairness tools that courts and regulators depend on?
4. If a volunteer can write the code but can't merge it, is the tool truly "open source" — or is it "open to the community but closed to the community"?
5. Should the name "AI Fairness 360" change if it can't actually measure fairness along intersecting axes?

---

## 🔴 Debate #3: The Missing-Value Contract — What Should a Fairness Tool Do When Data Is Incomplete?

**Source:** [fairlearn/fairlearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Opened:** September 9, 2026  |  **Last updated:** September 16, 2026  |  **Status:** Open  |  **💬 Comments:** 3  
**Issue author:** [aiedwardyi](https://github.com/aiedwardyi)

### The Question

`MetricFrame` and `plot_roc_curve_by_group` handle missing sensitive feature values differently. When the data doesn't include race, gender, or another protected attribute, should the tool:

- **Option A — Fail loudly** and tell the user "you can't compute fairness without knowing who's in the group"?
- **Option B — Proceed silently** and compute what it can, perhaps with a warning?
- **Option C — Impute or estimate** the missing values using statistical methods?

The disagreement between MetricFrame and the ROC curve plotting function suggests that Fairlearn's own maintainers haven't agreed on the answer.

### Why It Matters

This question reveals a fundamental tension in fairness tooling:

1. **Strictness vs. usability.** A strict tool that refuses to run without complete data is more principled but less useful in the real world, where data is often messy.
2. **Silence vs. transparency.** If a tool proceeds silently with missing data, the user might not realize the fairness assessment is incomplete. If it warns, the user might ignore the warning.
3. **Who bears the risk?** If a tool imputes missing values and gets it wrong, whose responsibility is the error? The tool developer? The data scientist? The regulator who accepted the tool's output?
4. **The philosophical question underneath.** A fairness tool requires sensitive data to detect bias. But in many contexts (hiring, lending, healthcare), collecting sensitive data is itself controversial. The tool's approach to missing data reveals its assumptions about whether fairness can be assessed without explicit group membership — and that's a political question, not a technical one.

### Discussion Prompts

1. Should fairness tools be strict or silent when sensitive data is missing?
2. Is it ethical for a fairness tool to produce an incomplete assessment without clearly flagging the gap?
3. Can you measure fairness without knowing who's being measured — and should you try?
4. Who decides what constitutes "missing" data? Is race missing because it wasn't collected, or because it was deliberately excluded?

---

## 🔴 Debate #4: The SHAP Measurement Problem — Does the Denominator Change the Story?

**Source:** [Fair-Code Issue #672](https://github.com/yakew7/Fair-Code/issues/672)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Status:** Open, unresolved

### The Question

When auditing racial bias in a COMPAS recidivism model using SHAP values, how should you report race's influence? The answer depends on a choice that isn't mentioned in the original write-up:

- **Option A — Top-5 features denominator:** Race accounts for **48.1%** of the top-5 features' combined influence
- **Option B — All-features denominator:** Race accounts for **42.3%** of all features' combined influence

Both numbers are correct. They answer different questions.

### Why It Matters

The choice of denominator is a rhetorical decision disguised as a number. "Race drives 48% of the model's decisions" sounds more alarming than "race drives 42%" — but the difference isn't about the model. It's about what you want the audience to feel.

In a courtroom, a prosecutor would cite 48%. A defense attorney would cite 42%. A journalist would pick whichever fits the headline.

### Discussion Prompts

1. Is there a "correct" way to aggregate feature importance for fairness reporting?
2. Should fairness audits standardize denominator conventions (like p-value thresholds)?
3. Does the choice of denominator change policy outcomes — and if so, who should make that choice?
4. Should fairness reports include both numbers, with an explanation of what each one means?

---

## 🔴 Debate #5: The Impossibility Triangle — Which Fairness Metric Wins?

**Source:** [Fair-Code Issue #665](https://github.com/yakew7/Fair-Code/issues/665)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Status:** Ongoing

### The Theorem

Chouldechova (2017) and Kleinberg et al. (2016) independently proved: **when base rates differ between groups, you cannot simultaneously satisfy equalized odds and predictive parity** — except in trivial edge cases.

### The Real-World Battle

The COMPAS case is the canonical illustration:

| Who | Metric Used | Finding |
|---|---|---|
| **ProPublica** | Equalized Odds (FPR parity) | Black defendants falsely flagged at 78.1% vs White at 0.3% — *unfair!* |
| **Northpointe** | Predictive Parity (PPV parity) | PPV 62.8% vs 50.0% — *tool is not biased!* |

Both were mathematically correct. They were measuring different things.

### Discussion Prompts

1. Should there be a "default" fairness metric for high-stakes domains — and who should set it?
2. Is the impossibility theorem an argument against fairness metrics altogether?
3. If you can't satisfy all metrics, whose rights should the metric protect?

---

## 🔗 Quick Links to All Live Debates

| # | Debate | Repo | Issue | Open Since | Status |
|---|---|---|---|---|---|
| 1 | What does "zero" mean? | AIF360 | [#528](https://github.com/Trusted-AI/AIF360/issues/528) | April 2024 | Open, unmaintained |
| 2 | Scalar vs. intersectional breakdown | AIF360 | [#558](https://github.com/Trusted-AI/AIF360/issues/558) | January 2026 | Open, volunteer waiting |
| 3 | Missing-value contract | FairLearn | [#1725](https://github.com/fairlearn/fairlearn/issues/1725) | September 2026 | Open, 3 comments |
| 4 | SHAP Measurement Problem | Fair-Code | [#672](https://github.com/yakew7/Fair-Code/issues/672) | Ongoing | Open |
| 5 | Impossibility Triangle | Fair-Code | [#665](https://github.com/yakew7/Fair-Code/issues/665) | Ongoing | Open |

---

## ✍️ How to Contribute a Debate

Found a fairness controversy in an open-source issue thread? Contribute:

1. Link the issue/PR
2. Summarize the two (or more) sides
3. Explain *why* the debate matters beyond the repo
4. Add discussion prompts for podcast listeners

Format: follow the structure above — **The Claim → Why It Matters → The Response → The Deeper Question → Discussion Prompts**

---

*Debates are sourced from real GitHub issue threads. All issues verified as open in September 2026. The summary represents a particular perspective; listener and contributor counterarguments are welcome — open an issue or submit a PR.*