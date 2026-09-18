# ⚖️ Ongoing Debates in Algorithmic Fairness

A curated summary of real controversies happening in open-source fairness projects — sourced from GitHub issue threads, explainer corrections, and reproduced-bug reports.

---

## Debate 1: The Average-Odds Documentation Bug — What Does "Zero" Mean in AIF360?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Project:** [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Tags:** `documentation`, `bug`  
**Status:** Open since April 23, 2024 — unresolved, 2 👍 reactions, 1 correction comment (September 11, 2026)  
**Author:** [AndreFCruz](https://github.com/AndreFCruz)  
**Volunteer:** [Hanabi9248](https://github.com/Hanabi9248) (comment, Sept 2026)

### The Bug Report

AndreFCruz filed this issue after noticing that the [AIF360 documentation](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> "A value of 0 indicates equality of odds."

for the `average_odds_difference` metric.

But that's mathematically wrong. As the issue's attached diagram shows, there are configurations where `average_odds_difference = 0` yet **equality of odds does not hold**. The metric formula and the equalized-odds criterion are measuring different things, and the documentation conflates them.

The same error appears on IBM's own fairness metrics explainer page.

### The Two Sides

**Side A — This is a documentation bug, not a conceptual one.** The metric itself is well-defined; the problem is that the docstring says "equality of odds" when it should say something like "a relaxation of equality of odds" or "average odds difference." Fix the words, not the math.

**Side B — The terminology matters because it shapes policy.** If AIF360 — the most widely used fairness toolkit in production and government — labels a metric as indicating "equality of odds" when it doesn't, then every audit report, regulatory filing, and court brief that cites this metric inherits the error. The documentation isn't just describing the tool; it's defining what "fairness" means in practice.

### The Community Response

After **17 months** with no maintainer response, Hanabi9248 volunteered in September 2026 with a focused correction:

- Fix the docstrings in `MetricTextExplainer` and its JSON output
- Provide a four-row example where `average_odds_difference = 0` while both `average_abs_odds_difference` and `equalized_odds_difference` = 1
- The metric formulas remain unchanged — only the descriptions and examples are corrected

But the issue remains open, unassigned, and unmerged.

### The Broader AIF360 Maintenance Picture

This documentation bug is not an isolated case. During research, we found several other open issues in the AIF360 repo that paint a picture of a project under strain:

- **[#548 — Website is down](https://github.com/Trusted-AI/AIF360/issues/548):** Open since February 2025 with 7 comments. The official documentation website has been unavailable, making it harder for users to access metric definitions. A dead website + a misleading docstring = a double documentation failure.

- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric for intersectional analysis, but the existing metrics still have documentation errors. This raises the question: should you add new metrics before fixing the old ones?

- **[#526 — Memory Management Issue in ClassificationMetric](https://github.com/Trusted-AI/AIF360/issues/526):** Open since April 2024. A core class has performance problems. If the foundation is shaky, the transactions (fairness measurements) built on it are suspect.

The pattern: **documentation errors, infrastructure decay, and performance issues** all coexist. Volunteer corrections are welcome but unassigned. The question isn't just "who fixes one docstring?" but "what is the maintenance model for a toolkit that regulators rely on?"

### Why It Matters Beyond the Repo

This reveals a structural tension in the fairness toolkit ecosystem:

1. **The gap between research definitions and production tooling.** Research papers define fairness metrics with mathematical precision. Toolkits wrap them in APIs with docstrings that simplify — and sometimes distort — the original definitions.

2. **Who maintains the definitions?** AIF360 is an IBM Research project. When a volunteer identifies a documentation error and no IBM maintainer responds for 17 months, the question isn't just "who fixes the docstring?" but "who owns the standard?"

3. **The AIF360 vs. FairLearn divergence.** FairLearn (Microsoft, 2,286 stars) provides overlapping metrics with AIF360 but may define or compute them differently. If two mainstream tools disagree on what "average odds difference = 0" means, practitioners and regulators have no single authoritative reference.

4. **The downstream harm.** Courts citing AIF360's metrics, regulators referencing its documentation, and engineers trusting its API — all inherit whatever the docstring says. A misleading docstring isn't an academic issue; it's a real-world harm vector.

5. **The volunteer maintenance trap.** Hanabi9248 offered a concrete fix, but the issue can't be merged without maintainer push. When the official maintainers are silent, does the volunteer's fix have any authority? Who merges it — and who guarantees it's correct?

**Discussion prompts for the episode:**
- Should fairness toolkits be required to publish formal verification of their metric definitions — the way cryptographic libraries publish formal proofs?
- Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
- If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
- Is 17 months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?
- When two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?
- Is a volunteer-submitted fix with no maintainer merge pathway actually authoritative? What does "open" really mean for an issue in a tool used by courts?

---

## Debate 2: The Intersectional Analysis Gap — Should Fairness Tools Return a Scalar or a Breakdown?

**Source:** [Trusted-AI/AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558)  
**Project:** [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Tags:** `enhancement`, `intersectionality`  
**Status:** Open since January 21, 2026 — unresolved, volunteer offered to implement (Sept 2026)

### The Question

The current implementation of the Empirical Differential Fairness (EDF) metric only returns a **single scalar value** summarizing fairness across protected attributes. But for intersectional analysis, a single number hides the story: which specific combinations of attribute groups contribute most to the maximum log-ratio?

jetverbeek asked for the metric to return:
- The pair(s) of attribute groups that produce the max log ratio
- A breakdown of all/top X group combinations and their respective log-ratios

### The Two Sides

**Side A — Single scalar is enough for most use cases.** Regulatory frameworks (EU AI Act, FTC guidance) typically ask for a single fairness score per protected attribute. Adding group-pair breakdowns complicates the API and makes it harder to produce the "one number" that regulators expect.

**Side B — Intersectional analysis is essential for real fairness.** A single scalar can hide severe disparities for specific subpopulations. Black women, disabled immigrants, Indigenous trans people — these intersections can be the worst-off groups, and a single scalar averaged over all groups canmask their specific harm. Hanabi9248 offered to implement a separate method for the group-pair breakdown while keeping the scalar return for backward compatibility.

### The Community Response

Hanabi9248 (September 2026) volunteered to implement the enhancement:
- Keep `smoothed_empirical_differential_fairness()` returning a scalar for backward compatibility
- Add a separate method for the group-pair breakdown, including pairs attaining the maximum and an optional top-k limit
- Use the same instance weights and Dirichlet smoothing as the existing metric
- Include tests for ties and multiple protected attributes

But like Issue #528, the issue remains **unassigned and unmerged** — the maintainers haven't confirmed or declined the contribution.

### Why It Matters Beyond the Repo

1. **The scalar vs. breakdown tension is a political choice.** A single "fairness score" simplifies regulation but can erase the communities that need the most protection. Intersectional analysis is more honest but harder to legislate.

2. **Who decides the granularity of fairness reporting?** Regulators, practitioners, or the communities being audited? A tool that only returns scalars makes the choice for the user — by default, it favors simplicity over precision.

3. **The volunteer-maintenance pattern.** Both AIF360 debates (#528 and #558) share the same pattern: a volunteer offers a fix or enhancement, but the issue sits unassigned. This raises questions about whether IBM's "open source" model is genuinely collaborative or merely decorative.

**Discussion prompts for the episode:**
- Should fairness tools be required to support intersectional analysis by default, or is a single scalar acceptable for regulatory contexts?
- If a single scalar can mask the worst-off group, is it ethical to ship that as the default output?
- Who should pay for the maintenance of fairness tools that courts and regulators depend on?
- Is a volunteer-submitted PR without maintainer review actually "open source," or is it just a suggestion box?

---

## Debate 3: The Missing-Value Contract — Should Fairness Tools Be Strict or Silent? (FairLearn #1725)

**Source:** [fairlearn/fairlearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Tags:** `bug`, `consistency`, `contract`  
**Status:** Open since September 2026 — maintainer-confirmed fix pending

### The Bug Report

`MetricFrame` raises a `ValueError` when given missing sensitive feature values, while `plot_roc_curve_by_group` **silently drops** the rows with missing values and draws curves for the remaining groups plus an "Overall" curve. The docstring and source comments both claim the plot should match `MetricFrame`'s behavior.

**Minimum reproduction:**
```python
import numpy as np
from sklearn.metrics import accuracy_score
from fairlearn.metrics import MetricFrame, plot_roc_curve_by_group

y_true = np.array([0, 1, 0, 1, 0, 1])
y_pred = np.array([0, 1, 1, 1, 0, 0])
y_score = np.array([0.1, 0.9, 0.8, 0.7, 0.2, 0.3])
sf = np.array(["a", "a", "b", "b", np.nan, np.nan], dtype=object)

MetricFrame(metrics=accuracy_score, y_true=y_true, y_pred=y_pred, sensitive_features=sf)
# → ValueError: Feature 'sensitive_feature_0' contains missing values.

plot_roc_curve_by_group(y_true=y_true, y_score=y_score, sensitive_features=sf)
# → draws Overall, "a", "b"; the two NaN rows are silently dropped
```

### The Two Sides

**Side A — Make the plot match MetricFrame (strict consistency).** Contributor LobsterQBA argued that `plot_roc_curve_by_group` should reject missing values just like `MetricFrame` does, because silently dropping rows creates a misleading comparison: the "Overall" curve includes all rows, but per-group curves exclude the missing-data rows.

**Side B — Keep the skip behavior and fix the docs (permissive pragmatic).** Dropping rows might be the right behavior for visualization — you can't plot a ROC curve for a group with no data — and the docs should just be upfront about it.

**Maintainer's verdict (romanlutz, Sept 16):**
> "Missing sensitive feature values should be rejected with a clear `ValueError` in both APIs, consistent with `MetricFrame` after #1698. We do not want the ROC helper to silently drop rows from group curves while retaining them in the overall curve."

The maintainer confirmed: **strict wins.** But the fix hasn't been merged yet.

### Why It Matters Beyond the Repo

1. **Should fairness tools be strict or permissive by default?** A strict `ValueError` protects users from invisible bias (you can't audit what you can't see), but it also blocks legitimate analyses where missing data is the norm.

2. **The "Overall vs. Groups" trap.** When a tool silently drops rows, the "Overall" curve includes everyone while group curves exclude some — creating a comparison that doesn't add up. This is a **visual deception** that could lead auditors to wrong conclusions.

3. **The contract question.** What should the *documented contract* of a fairness function be? Should it say "this will raise an error" or "this will handle missing data gracefully"?

**Discussion prompts:**
- Should fairness tools raise errors on missing sensitive data, or should they handle it gracefully?
- Is silent row-dropping a bug or a feature? When would you want to skip missing data vs. reject it?
- If two functions in the same toolkit have different missing-data behaviors, is that a bug — or a design choice?

---

## Debate 4: The SHAP Measurement Problem — Does the Denominator Change the Story? (Fair-Code #672)

**Source:** [Fair-Code Issue #672](https://github.com/yakew7/Fair-Code/issues/672)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Tags:** `documentation`, `good first issue`  
**Status:** Open, unresolved

### The Question

When auditing racial bias in a COMPAS recidivism model using SHAP values, how should you report race's influence? The answer depends on a choice that isn't mentioned in the original write-up:

- **Option A — Top-5 features denominator:** Race accounts for **48.1%** of the top-5 features' combined influence
- **Option B — All-features denominator:** Race accounts for **42.3%** of all features' combined influence

Both numbers are correct. They answer different questions.

### Why It Matters

The choice of denominator is a rhetorical decision disguised as a number. "Race drives 48% of the model's decisions" sounds more alarming than "race drives 42%" — but the difference isn't about the model. It's about what you want the audience to feel.

In a courtroom, a prosecutor would cite 48%. A defense attorney would cite 42%. A journalist would pick whichever fits the headline.

**Discussion prompts:**
- Is there a "correct" way to aggregate feature importance for fairness reporting?
- Should fairness audits standardize denominator conventions (like p-value thresholds)?
- Does the choice of denominator change policy outcomes — and if so, who should make that choice?

---

## Debate 5: The Impossibility Triangle — Which Fairness Metric Wins?

**Source:** Fair-Code Explainer + [Issue #665](https://github.com/yakew7/Fair-Code/issues/665)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Tags:** `documentation`, `bug`  
**Status:** Ongoing — the explainer is widely cited, its numeric examples are contested

### The Theorem

Chouldechova (2017) and Kleinberg et al. (2016) independently proved: **when base rates differ between groups, you cannot simultaneously satisfy equalized odds and predictive parity** — except in trivial edge cases.

### The Real-World Battle

The COMPAS case is the canonical illustration:

| Who | Metric Used | Finding |
|---|---|---|
| **ProPublica** | Equalized Odds (FPR parity) | Black defendants falsely flagged at 78.1% vs White at 0.3% — *unfair!* |
| **Northpointe** | Predictive Parity (PPV parity) | PPV 62.8% vs 50.0% — *tool is not biased!* |

Both were mathematically correct. They were measuring different things.

But Fair-Code's own issues reveal that even the **numbers used to illustrate this famous case** don't always match what the repo's own code produces — raising the question: if the textbook example has contested numerics, how solid is the consensus on which metric to use?

**Discussion prompts:**
- Should there be a "default" fairness metric for high-stakes domains — and who should set it?
- Is the impossibility theorem an argument against fairness metrics altogether?
- If you can't satisfy all metrics, whose rights should the metric protect?

---

## How to Contribute a Debate

Found a great fairness controversy in an open-source issue thread? Contribute:

1. Link the issue/PR
2. Summarize the two (or more) sides
3. Explain *why* the debate matters beyond the repo
4. Add discussion prompts for our listeners

Format: follow the structure above — **The Question → Why It Matters → Discussion Prompts**

---

*Debates are sourced from real GitHub issue threads. The summary represents the maintainer's perspective; listener and contributor counterarguments are welcome — open an issue or submit a PR.*