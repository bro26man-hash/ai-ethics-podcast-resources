# ⚖️ Ongoing Debates in Algorithmic Fairness

A curated summary of real controversies happening in open-source fairness projects — sourced from GitHub issue threads, explainer corrections, and reproduced-bug reports.

---

## Debate 1: The Missing-Value Contract — Should Fairness Tools Be Strict or Silent?

**Source:** [fairlearn/fairlearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Tags:** `bug`, `consistency`, `contract`  
**Status:** Open since September 9, 2026 — unresolved, maintainer-confirmed fix pending  
**Author:** [aiedwardyi](https://github.com/aiedwardyi) (contributor)  
**Maintainer confirmation:** [romanlutz](https://github.com/romanlutz) (Sept 16, 2026)

### The Bug Report

`MetricFrame` raises a `ValueError` when given missing sensitive feature values, while `plot_roc_curve_by_group` **silently drops** the rows with missing values and draws curves for the remaining groups plus an "Overall" curve. The docstring and source comments at `_roc_auc.py` L124-130 both claim the plot should match `MetricFrame`'s behavior. That was true when PR #1713 landed, then PR #1698 made `MetricFrame` raise. Both are unreleased and slated for v0.15.0.

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
#   Remove or replace them before constructing a MetricFrame

plot_roc_curve_by_group(y_true=y_true, y_score=y_score, sensitive_features=sf)
# → draws Overall, "a", "b"; the two NaN rows are silently dropped
```

### The Two Sides

**Side A — Make the plot match MetricFrame (strict consistency).**  
Contributor [LobsterQBA](https://github.com/LobsterQBA) (Sept 14) argued that `plot_roc_curve_by_group` should reject missing values just like `MetricFrame` does, because silently dropping rows creates a misleading comparison: the "Overall" curve includes all rows, but per-group curves exclude the missing-data rows. That means `Overall ≠ weighted average of groups`, which is a silent inconsistency that could distort fairness analysis.

**Side B — Keep the skip behavior and fix the docs (permissive pragmatic).**  
An alternative view (implied in the issue body) is that dropping rows might be the right behavior for visualization — you can't plot a ROC curve for a group with no data — and the docs should just be upfront about it. The maintainer didn't take this side, but it's a reasonable design position.

**Maintainer's verdict (romanlutz, Sept 16):**  
> "Missing sensitive feature values should be rejected with a clear `ValueError` in both APIs, consistent with `MetricFrame` after #1698. We do not want the ROC helper to silently drop rows from group curves while retaining them in the overall curve."

The maintainer confirmed: **strict wins.** But the fix hasn't been merged yet.

### Why It Matters Beyond the Repo

This is not just a consistency bug — it reveals a **deep design philosophical question** about fairness tools:

1. **Should fairness tools be strict or permissive by default?** A strict `ValueError` protects users from invisible bias (you can't audit what you can't see), but it also blocks legitimate analyses where missing data is the norm (e.g., survey data, real-world audits where sensitive attributes are often unrecorded).

2. **The "Overall vs. Groups" trap.** When a tool silently drops rows, the "Overall" curve includes everyone while group curves exclude some — creating a comparison that doesn't add up. This is a **visual deception** that could lead auditors to wrong conclusions.

3. **The contract question.** What should the *documented contract* of a fairness function be? Should it say "this will raise an error" or "this will handle missing data gracefully"? The FairLearn maintainers chose strictness — but other tools (Aequitas, AIF360) may make different choices.

4. **Who decides what "correct" behavior is?** The issue was filed by a contributor, confirmed by a maintainer, but the fix is still pending. Meanwhile, anyone using the dev version could get inconsistent results depending on which function they call.

**Discussion prompts for the episode:**
- Should fairness tools raise errors on missing sensitive data, or should they handle it gracefully? What are the trade-offs?
- Is silent row-dropping a bug or a feature? When would you want to skip missing data vs. reject it?
- If two functions in the same toolkit have different missing-data behaviors, is that a bug — or a design choice that needs better documentation?
- Should fairness toolkits publish formal "contracts" (like API specifications) that define exactly what each function does with edge cases?
- Who bears the risk when a fairness tool's behavior is inconsistent — the auditor, the regulator, or the person affected by the decision?

---

## Debate 2: The Average-Odds Documentation Bug — What Does "Zero" Mean in AIF360?

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

- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric, but the existing metrics still have documentation errors. This raises the question: should you add new metrics before fixing the old ones?

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

## Debate 3: The SHAP Measurement Problem — Does the Denominator Change the Story?

**Source:** [Fair-Code Issue #672](https://github.com/yakew7/Fair-Code/issues/672)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Tags:** `documentation`, `good first issue`  
**Status:** Open, unresolved

### The Question

When auditing racial bias in a COMPAS recidivism model using SHAP values, how should you report race's influence? The answer depends on a choice that isn't mentioned in the original write-up:

- **Option A — Top-5 features denominator:** Race accounts for 0.1066 / (0.1066 + 0.0624 + 0.0234 + 0.0165 + 0.0126) = **48.1%** of the top-5 features' combined influence
- **Option B — All-features denominator:** Race accounts for 0.1066 / 0.2523 = **42.3%** of all features' combined influence (rounds to "roughly 40%")

Both numbers are correct. They answer different questions.

### Why It Matters

The choice of denominator is a rhetorical decision disguised as a number. "Race drives 48% of the model's decisions" sounds more alarming than "race drives 42% of the model's decisions" — but the difference isn't about the model. It's about what you want the audience to feel.

In a courtroom, a prosecutor would cite 48%. A defense attorney would cite 42%. A journalist would pick whichever fits the headline.

### The Deep Problem

This isn't just about SHAP. It's about **who gets to decide what number tells the story of bias** — and whether there's a "correct" denominator or whether every choice embeds a value judgment about what counts as "influence."

**Discussion prompts for the episode:**
- Is there a "correct" way to aggregate feature importance for fairness reporting?
- Should fairness audits standardize denominator conventions (like p-value thresholds)?
- Does the choice of denominator change policy outcomes — and if so, who should make that choice?

---

## Debate 4: The Counterfactual Fairness Reversal — When the Reproduction Contradicts the Argument

**Source:** [Fair-Code Issue #654](https://github.com/yakew7/Fair-Code/issues/654)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Tags:** `bug`, `documentation`  
**Status:** Open, unresolved

### The Claim

The [Counterfactual Fairness explainer](https://github.com/yakew7/Fair-Code/blob/main/explainers/counterfactual-fairness.md) presents a synthetic lending audit where:

- 35.7% of applicants would get a different loan decision if they had been born into the other racial group
- White applicants flip at 24.4%, Black applicants flip at 52.0%
- The rhetorical point: *"this is the operational signature of race-based decisions — Black defendants flip much more often than White ones"*

### The Reality

When the author reproduced the exact code (same seed, same model, same pipeline):

- Violation rate: **31.5%**, not 35.7%
- White applicants flip at **32.1%**, Black applicants flip at **30.6%**
- The groups flip at **nearly identical rates** — not the dramatic 52% vs 24.4% disparity the article claims

### Why It's a Real Debate, Not Just a Bug

This isn't a typo. It's a **structural problem in how counterfactual fairness audits are written**:

1. **The narrative preceded the evidence.** The article's argument — "race-based decisions harm Black applicants more" — was written first. The code was assembled to illustrate it. When the code produced a different result, the article kept its rhetorical framing and the issue got filed as a "bug."

2. **Simulation variance vs. rhetorical conviction.** The author labels the discrepancy as a numeric error, but the deeper issue is: **who gets to write the narrative of algorithmic harm?** The person who runs the audit, or the person who writes the explainer?

3. **The general lesson.** If a fairness explainer can get its headline numbers wrong while being "technically reproducible," what does that mean for the thousands of blog posts, conference talks, and policy briefs that cite fairness statistics without reproduction checks?

**Discussion prompts for the episode:**
- Should fairness explainers be required to include reproduction code and output blocks?
- Is there a difference between a "conceptual illustration" and a "reproducible audit" — and should one be labeled as the other?
- When a reproduction contradicts the original claim, who gets to tell the corrected story?

---

## Debate 5: The Impossibility Triangle — Which Fairness Metric Wins?

**Source:** [Fair-Code Explainer: Why Fairness Metrics Conflict](https://github.com/yakew7/Fair-Code/blob/main/explainers/fairness-metric-conflicts.md) + [Issue #665](https://github.com/yakew7/Fair-Code/issues/665)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Tags:** `documentation`, `bug` (issues filing corrections to the explainer's COMPAS numbers)  
**Status:** Ongoing — the explainer is widely cited, its numeric examples are contested

### The Theorem

Chouldechova (2017) and Kleinberg et al. (2016) independently proved: **when base rates differ between groups, you cannot simultaneously satisfy equalized odds and predictive parity** — except in trivial edge cases.

This isn't a modeling flaw. It's an identity that follows from the definitions.

### The Real-World Battle

The COMPAS case is the canonical illustration:

| Who | Metric Used | Finding |
|---|---|---|
| **ProPublica** | Equalized Odds (FPR parity) | Black defendants falsely flagged at 78.1% vs White at 0.3% — *unfair!* |
| **Northpointe** | Predictive Parity (PPV parity) | PPV 62.8% vs 50.0% — *closer than the error-rate gap, tool is not biased!* |

Both were mathematically correct. They were measuring different things.

But the Fair-Code repo's own issues (#665, #671) reveal that even the **numbers used to illustrate this famous case** don't always match what the repo's own code produces — raising the question: if the textbook example has contested numerics, how solid is the consensus on which metric to use?

### The Deeper Question

Choosing a fairness metric is not a technical decision — it's an **ethical and political** one:

| Metric | Prioritizes | Who bears the cost when it fails |
|---|---|---|
| Demographic Parity | Equal access to outcomes | Groups with higher true rates may be under-predicted |
| Equalized Odds | Equal error rates | Accuracy per group may be sacrificed |
| Predictive Parity | Equal prediction reliability | Groups with lower base rates face higher false positive rates |

**Who decides which metric a court uses? Who picks the one that regulators enforce? Who bears the cost when the wrong one is chosen?**

**Discussion prompts for the episode:**
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