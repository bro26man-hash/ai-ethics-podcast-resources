# ⚖️ Ongoing Debates in Algorithmic Fairness

A curated summary of **real controversies happening in open-source fairness projects** — sourced from GitHub issue threads, explainer corrections, and reproduced-bug reports. Each debate is grounded in a specific, unresolved issue thread that listeners can read and follow.

---

## Debate 1: What Does "Zero" Mean? The AIF360 Average-Odds Documentation Bug

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Project:** [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Tags:** `documentation`, `bug`  
**Status:** Open since **April 23, 2024** — unresolved for **17 months**, 2 👍 reactions  
**Author:** [AndreFCruz](https://github.com/AndreFCruz)  
**Volunteer:** [Hanabi9248](https://github.com/Hanabi9248) (comment, Sept 2026)

### The Claim

AndreFCruz, a mathematician, filed this issue after noticing that the [AIF360 documentation](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> *"A value of 0 indicates equality of odds."*

for the `average_odds_difference` metric.

But that's **mathematically wrong**. As the issue's attached diagram demonstrates, there are configurations where `average_odds_difference = 0` yet **equality of odds does not hold**. The metric formula and the equalized-odds criterion are measuring different things, and the documentation conflates them. The same error appears on IBM's own fairness metrics explainer page.

### The Two Sides

**Side A — This is a documentation bug, not a conceptual one.**  
The metric itself is well-defined; the problem is that the docstring says "equality of odds" when it should say something like "a relaxation of equality of odds" or simply "average odds difference." Fix the words, not the math.

**Side B — The terminology matters because it shapes policy.**  
If AIF360 — the most widely used fairness toolkit in production and government — labels a metric as indicating "equality of odds" when it doesn't, then every audit report, regulatory filing, and court brief that cites this metric inherits the error. The documentation isn't just describing the tool; it's **defining what "fairness" means in practice**.

### The Community Response

After **17 months** with no maintainer response, Hanabi9248 volunteered in September 2026 with a focused correction:

- Fix the docstrings in `MetricTextExplainer` and its JSON output
- Provide a four-row example where `average_odds_difference = 0` while both `average_abs_odds_difference` and `equalized_odds_difference` = 1
- The metric formulas remain unchanged — only the descriptions and examples are corrected

But the issue remains **open, unassigned, and unmerged**.

### The Broader AIF360 Maintenance Picture

This documentation bug is not an isolated case. During research, we found several other open issues in the AIF360 repo that paint a picture of a project under strain:

- **[#548 — Website is down](https://github.com/Trusted-AI/AIF360/issues/548):** Open since February 2025 with 7 comments. The official documentation website has been unavailable, making it harder for users to access metric definitions. A dead website + a misleading docstring = a double documentation failure.
- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric, but the existing metrics still have documentation errors. This raises the question: should you add new metrics before fixing the old ones?
- **[#526 — Memory Management Issue in ClassificationMetric](https://github.com/Trusted-AI/AIF360/issues/526):** Open since April 2024. A core class has performance problems. If the foundation is shaky, the transactions (fairness measurements) built on it are suspect.

The pattern: **documentation errors, infrastructure decay, and performance issues** all coexist. Volunteer corrections are welcome but unassigned. The question isn't just "who fixes one docstring?" but "what is the maintenance model for a toolkit that regulators rely on?"

### Why It Matters Beyond the Repo

1. **The gap between research definitions and production tooling.** Research papers define fairness metrics with mathematical precision. Toolkits wrap them in APIs with docstrings that simplify — and sometimes distort — the original definitions.
2. **Who maintains the definitions?** AIF360 is an IBM Research project. When a volunteer identifies a documentation error and no IBM maintainer responds for 17 months, the question isn't just "who fixes the docstring?" but "who owns the standard?"
3. **The downstream harm.** Courts citing AIF360's metrics, regulators referencing its documentation, and engineers trusting its API — all inherit whatever the docstring says. A misleading docstring isn't an academic issue; it's a real-world harm vector.
4. **The volunteer maintenance trap.** Hanabi9248 offered a concrete fix, but the issue can't be merged without maintainer push. When the official maintainers are silent, does the volunteer's fix have any authority?

### 🎙️ Discussion Questions

- Should fairness toolkits be required to publish formal verification of their metric definitions — the way cryptographic libraries publish formal proofs?
- Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
- If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
- Is 17 months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?
- When two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?

---

## Debate 2: Should Fairness Tools Serve Intersectional Analysis?

**Source:** [Trusted-AI/AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558)  
**Project:** [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Tags:** `enhancement`, `intersectionality`  
**Status:** Open since **January 21, 2026** — unresolved  
**Author:** [jetverbeek](https://github.com/jetverbeek)  
**Volunteer:** [Hanabi9248](https://github.com/Hanabi9248) (comment, Sept 2026)

### The Claim

jetverbeek argues that the current Empirical Differential Fairness (EDF) metric returns only a **single scalar** summarizing fairness across all protected attributes. This makes it impossible to identify *which specific combinations of group attributes* are driving discrimination.

The request: extend EDF to return:
- The pair(s) of attribute groups that produce the maximum log-ratio
- A breakdown of all (or top-X) group combinations and their respective log-ratios

### The Two Sides

**Side A — A single scalar is enough for regulators.**  
Aggregate metrics are easier to compute, easier to report, and what regulators often want. A single "fairness score" can be added to a compliance checklist. Intersectional analysis adds complexity without clear regulatory benefit.

**Side B — A single scalar erases which communities are most harmed.**  
Discrimination rarely operates on a single axis. A Black woman faces compounding biases that a "race" metric and a "gender" metric separately would miss entirely. Intersectional analysis is what marginalized communities actually experience. If the tool only returns a scalar, the most harmed groups may be invisible in the aggregate.

### The Community Response

Hanabi9248 proposed a pragmatic compromise:

- Keep `smoothed_empirical_differential_fairness()` returning a scalar (for backward compatibility and regulatory reporting)
- Add a **separate method** for the group-pair breakdown, including the pairs attaining the maximum and an optional top-k limit
- Use the same instance weights and Dirichlet smoothing as the existing metric
- Include tests for ties and multiple protected attributes

This approach respects both needs — but raises the question of whether two separate tools will actually be used together, or whether practitioners will default to the simpler one.

### Why It Matters Beyond the Repo

1. **Is a single fairness number a form of erasure?** When a tool returns only an aggregate, it can hide the fact that a very small group is being disproportionately harmed. The "average" can be fair while the "specifics" are not.
2. **Who decides what to measure?** Intersectional analysis requires deciding which attribute combinations to test. This is a political decision disguised as a technical one — and it may reveal findings that are uncomfortable for the institutions commissioning the audit.
3. **The computational complexity barrier.** Intersectional analysis grows combinatorially with the number of protected attributes. For 5 attributes with 3 categories each, there are 243 possible group combinations. Does the tool scale, or does it force practitioners to pre-filter — and thus pre-decide — which intersections matter?
4. **The "so what?" problem.** Even if you identify that group X is most harmed, what do you do about it? The metric doesn't prescribe a remediation. It just tells you where to look. Is identification enough, or should fairness tools also recommend fixes?

### 🎙️ Discussion Questions

- Is a single fairness number a form of erasure — hiding which groups are most harmed?
- Should fairness tools be designed for the communities they affect, or for the institutions that audit them?
- What happens when an intersectional breakdown reveals that your model is most harmful to a very small group — do you keep deploying it?
- Should intersectional analysis be mandatory in fairness audits, or is it a "nice to have" that adds complexity without clear benefit?
- Who bears the cost when an intersectional analysis reveals an uncomfortable truth — the auditor, the institution, or the affected community?

---

## Debate 3: The Missing-Value Contract — Should Fairness Tools Be Strict or Silent?

**Source:** [fairlearn/fairlearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Tags:** `bug`, `consistency`, `contract`  
**Status:** Open since September 9, 2026 — unresolved, maintainer-confirmed fix pending  
**Author:** [aiedwardyi](https://github.com/aiedwardyi)  
**Maintainer confirmation:** [romanlutz](https://github.com/romanlutz) (Sept 16, 2026)

### The Bug Report

`MetricFrame` raises a `ValueError` when given missing sensitive feature values, while `plot_roc_curve_by_group` **silently drops** the rows with missing values and draws curves for the remaining groups plus an "Overall" curve. The docstring and source comments both claim the plot should match `MetricFrame`'s behavior. That was true when PR #1713 landed, then PR #1698 made `MetricFrame` raise. Both are unreleased and slated for v0.15.0.

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
# → ValueError: Feature contains missing values.

plot_roc_curve_by_group(y_true=y_true, y_score=y_score, sensitive_features=sf)
# → draws Overall, "a", "b"; the two NaN rows are silently dropped
```

### The Two Sides

**Side A — Make the plot match MetricFrame (strict consistency).**  
Silently dropping rows creates a misleading comparison: the "Overall" curve includes all rows, but per-group curves exclude the missing-data rows. That means `Overall ≠ weighted average of groups`, which is a silent inconsistency that could distort fairness analysis.

**Side B — Keep the skip behavior and fix the docs (permissive pragmatic).**  
Dropping rows might be the right behavior for visualization — you can't plot a ROC curve for a group with no data — and the docs should just be upfront about it.

**Maintainer's verdict (romanlutz, Sept 16):**  
> "Missing sensitive feature values should be rejected with a clear `ValueError` in both APIs, consistent with `MetricFrame` after #1698. We do not want the ROC helper to silently drop rows from group curves while retaining them in the overall curve."

The maintainer confirmed: **strict wins.** But the fix hasn't been merged yet.

### Why It Matters Beyond the Repo

1. **Should fairness tools be strict or permissive by default?** A strict `ValueError` protects users from invisible bias, but it also blocks legitimate analyses where missing data is the norm (e.g., survey data, real-world audits where sensitive attributes are often unrecorded).
2. **The "Overall vs. Groups" trap.** When a tool silently drops rows, the "Overall" curve includes everyone while group curves exclude some — creating a comparison that doesn't add up. This is a **visual deception** that could lead auditors to wrong conclusions.
3. **The contract question.** What should the *documented contract* of a fairness function be? Should it say "this will raise an error" or "this will handle missing data gracefully"?
4. **Who decides what "correct" behavior is?** The issue was filed by a contributor, confirmed by a maintainer, but the fix is still pending. Meanwhile, anyone using the dev version could get inconsistent results depending on which function they call.

### 🎙️ Discussion Questions

- Should fairness tools raise errors on missing sensitive data, or should they handle it gracefully? What are the trade-offs?
- Is silent row-dropping a bug or a feature? When would you want to skip missing data vs. reject it?
- If two functions in the same toolkit have different missing-data behaviors, is that a bug — or a design choice that needs better documentation?
- Should fairness toolkits publish formal "contracts" (like API specifications) that define exactly what each function does with edge cases?
- Who bears the risk when a fairness tool's behavior is inconsistent — the auditor, the regulator, or the person affected by the decision?

---

## 🔍 How to Find Your Own Debate

The richest fairness debates are hiding in open issue threads right now. Here's how to find them:

1. **Search GitHub Issues** in fairness repos for labels like `bug`, `enhancement`, `discussion`, or `question`
2. **Look for issues with reactions** — 👍 indicates community agreement; 🤔 indicates confusion or disagreement
3. **Check for "good first issue" labels** — these often surface fundamental questions that newcomers ask
4. **Watch for issues that have been open for months** — if a simple fix were obvious, it would have been merged
5. **Read the comments, not just the issue body** — the real debate is in the back-and-forth between contributors and maintainers

*Contributors: Found a great fairness debate? Summarize it in this file following the structure above — The Claim → The Two Sides → The Community Response → Why It Matters → Discussion Questions.*