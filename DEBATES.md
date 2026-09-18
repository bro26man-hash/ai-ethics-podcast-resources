# ⚖️ Ongoing Debates in Algorithmic Fairness

A curated summary of real controversies happening in open-source fairness projects — sourced from GitHub issue threads, explainer corrections, and reproduced-bug reports.

---

## Debate 1: The Average-Odds Documentation Bug — What Does "Zero" Mean in AIF360?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Project:** [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Tags:** `documentation`, `bug`, `metric-definition`  
**Status:** Open since April 23, 2024 — unresolved, 2 👍 reactions, 1 correction comment (September 11, 2026)  
**Author:** [AndreFCruz](https://github.com/AndreFCruz)  
**Volunteer:** [Hanabi9248](https://github.com/Hanabi9248) (comment, Sept 2026)

### The Bug Report

AndreFCruz filed this issue after noticing that the [AIF360 documentation](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> "A value of 0 indicates equality of odds."

for the `average_odds_difference` metric.

But that's mathematically wrong. As the issue's attached diagram shows, there are configurations where `average_odds_difference = 0` yet **equality of odds does not hold**. The metric formula and the equalized-odds criterion are measuring different things, and the documentation conflates them.

The same error appears on IBM's own fairness metrics explainer page.

### The Counterexample

Hanabi9248 provided a concrete four-row dataset where:
- `average_odds_difference = 0`
- `average_abs_odds_difference = 1`
- `equalized_odds_difference = 1`

This proves that the metric formula is well-defined but the docstring's claim — "a value of 0 indicates equality of odds" — is false. The metric is a relaxation or approximation, not an exact indicator.

### The Two Sides

**Side A — This is a documentation bug, not a conceptual one.** The metric itself is well-defined; the problem is that the docstring says "equality of odds" when it should say something like "a relaxation of equality of odds" or "average odds difference." Fix the words, not the math. The metric formulas remain unchanged.

**Side B — The terminology matters because it shapes policy.** If AIF360 — the most widely used fairness toolkit in production and government — labels a metric as indicating "equality of odds" when it doesn't, then every audit report, regulatory filing, and court brief that cites this metric inherits the error. The documentation isn't just describing the tool; it's defining what "fairness" means in practice. A wrong docstring isn't a typo — it's a misdefinition with downstream consequences.

### The Community Response

After **17 months** with no maintainer response, Hanabi9248 volunteered in September 2026 with a focused correction:

- Fix the docstrings in `MetricTextExplainer` and its JSON output
- Provide a four-row example where `average_odds_difference = 0` while both `average_abs_odds_difference` and `equalized_odds_difference` = 1
- The metric formulas remain unchanged — only the descriptions and examples are corrected

But the issue remains open, unassigned, and unmerged. The correction exists as a comment, not a PR, and nobody with merge authority has picked it up.

### The Broader AIF360 Maintenance Picture

This documentation bug is not an isolated case. During research, we found several other open issues that paint a picture of a project under strain:

- **[#548 — Website is down](https://github.com/Trusted-AI/AIF360/issues/548):** Open since February 2025 with 7 comments. The official documentation website has been unavailable, making it harder for users to access metric definitions. A dead website + a misleading docstring = a double documentation failure.
- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric for intersectional analysis, but the existing metrics still have documentation errors. This raises the question: should you add new metrics before fixing the old ones?
- **[#526 — Memory Management Issue in ClassificationMetric](https://github.com/Trusted-AI/AIF360/issues/526):** Open since April 2024. A core class has performance problems.

The pattern: **documentation errors, infrastructure decay, and performance issues** all coexist. Volunteer corrections are welcome but unassigned.

### Why It Matters Beyond the Repo

1. **The gap between research definitions and production tooling.** Research papers define fairness metrics with mathematical precision. Toolkits wrap them in APIs with docstrings that simplify — and sometimes distort — the original definitions.

2. **Who maintains the definitions?** AIF360 is an IBM Research project. When a volunteer identifies a documentation error and no IBM maintainer responds for 17 months, the question isn't just "who fixes the docstring?" but "who owns the standard?"

3. **The AIF360 vs. FairLearn divergence.** FairLearn (Microsoft, 2,286 stars) provides overlapping metrics with AIF360 but may define or compute them differently. If two mainstream tools disagree on what "average odds difference = 0" means, practitioners and regulators have no single authoritative reference.

4. **The downstream harm.** Courts citing AIF360's metrics, regulators referencing its documentation, and engineers trusting its API — all inherit whatever the docstring says.

5. **The volunteer maintenance trap.** Hanabi9248 offered a concrete fix, but the issue can't be merged without maintainer push. The "open" in "open source" becomes questionable when the pathway from volunteer correction to merged fix is blocked.

**Discussion prompts for the episode:**
- Should fairness toolkits be required to publish formal verification of their metric definitions — the way cryptographic libraries publish formal proofs?
- Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
- If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
- Is 17 months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?
- When two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?
- Is a volunteer-submitted fix with no maintainer merge pathway actually authoritative?
- Should fairness tool maintainers be required to respond to issues within a certain timeframe? What would that look like?

---

## Debate 2: The Missing-Value Contract — Should Fairness Tools Be Strict or Silent?

**Source:** [fairlearn/fairlearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Tags:** `bug`, `consistency`, `contract`  
**Status:** Open since September 9, 2026 — maintainer-confirmed fix pending  
**Author:** [aiedwardyi](https://github.com/aiedwardyi) (CONTRIBUTOR)  
**Key participants:** LobsterQBA (contributor), romanlutz (maintainer)

### The Bug Report

`MetricFrame` raises a `ValueError` when given missing sensitive feature values, while `plot_roc_curve_by_group` **silently drops** the rows with missing values and draws curves for the remaining groups plus an "Overall" curve.

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

**Side A — Make the plot match MetricFrame (strict consistency).** Contributor LobsterQBA argued that `plot_roc_curve_by_group` should reject missing values just like `MetricFrame` does. "We do not want the ROC helper to silently drop rows from group curves while retaining them in the overall curve." This avoids comparing an overall population with per-group curves from a smaller population.

**Side B — Keep the skip behavior and fix the docs (permissive pragmatic).** Dropping rows might be the right behavior for visualization. If the plotting function is intentionally supposed to keep its skip behavior, the fix should be documentation that explicitly distinguishes it from `MetricFrame`.

**Maintainer's verdict (romanlutz, Sept 16, 2026):** "Missing sensitive feature values should be rejected with a clear `ValueError` in both APIs, consistent with MetricFrame after #1698." The maintainer confirmed: both should raise. But the fix is still pending — aiedwardyi has offered to open the PR, and LobsterQBA is ready to implement once the maintainer confirms scope.

### Why It Matters

1. **Should fairness tools be strict or permissive by default?** A strict `ValueError` protects users from invisible bias, but it also blocks legitimate analyses where missing data is the norm. In real-world datasets, missing sensitive attributes are common — especially when collecting demographic data is legally restricted or culturally sensitive.

2. **The "Overall vs. Groups" trap.** When a tool silently drops rows, the "Overall" curve includes everyone while group curves exclude some — creating a comparison that doesn't add up. This isn't just a bug; it's a statistical error that could lead to wrong conclusions about fairness.

3. **The contract question.** What should the *documented contract* of a fairness function be? If two functions in the same toolkit have different missing-data behaviors, is that a bug — or a design choice? And who decides?

4. **The consistency principle.** FairLearn's own documentation claimed the two APIs agreed. When they don't, users can't trust either one. The fix isn't just technical — it's about whether the toolkit presents itself as a coherent system or a collection of functions with inconsistent behavior.

**Discussion prompts:**
- Should fairness tools raise errors on missing sensitive data, or should they handle it gracefully?
- Is silent row-dropping a bug or a feature? When would you want to skip missing data vs. reject it?
- If two functions in the same toolkit have different missing-data behaviors, is that a bug — or a design choice?
- Who should decide the "contract" of a fairness function — the maintainers, the users, or the regulatory framework?
- Does the strict-permissive spectrum apply differently to auditing tools (where you want maximum strictness) vs. exploratory tools (where you want flexibility)?

---

## Debate 3: The Intersectional Analysis Gap — Should Fairness Tools Return a Scalar or a Breakdown?

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

**Side A — Single scalar is enough for most use cases.** Regulatory frameworks (EU AI Act, FTC guidance) typically ask for a single fairness score per protected attribute.

**Side B — Intersectional analysis is essential for real fairness.** A single scalar can hide severe disparities for specific subpopulations. Hanabi9248 offered to implement a separate method for the group-pair breakdown while keeping the scalar return for backward compatibility.

**Community response:** Hanabi9248 (September 2026) volunteered to implement the enhancement — but the issue remains unassigned and unmerged.

### Why It Matters

1. **The scalar vs. breakdown tension is a political choice.** A single "fairness score" simplifies regulation but can erase the communities that need the most protection.
2. **Who decides the granularity of fairness reporting?** Regulators, practitioners, or the communities being audited?
3. **The volunteer-maintenance pattern.** Both AIF360 debates (#528 and #558) share the same pattern: a volunteer offers a fix or enhancement, but the issue sits unassigned.

**Discussion prompts:**
- Should fairness tools be required to support intersectional analysis by default, or is a single scalar acceptable for regulatory contexts?
- If a single scalar can mask the worst-off group, is it ethical to ship that as the default output?
- Who should pay for the maintenance of fairness tools that courts and regulators depend on?

---

## Debate 4: The Generalist-vs-Specialist Tension — Who Does MetricFrame Serve?

**Source:** [fairlearn/fairlearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Tags:** `API`, `design-philosophy`  
**Status:** Open since April 22, 2021 — unresolved, 74 comments

### The Core Question

`MetricFrame` is FairLearn's flagship tool for computing fairness metrics with disaggregation by sensitive features. But it only works with metrics that have the signature `metric(y_true, y_pred)`. Maintainer MiroDudik proposes two changes:

- **Step 1:** Make all arguments keyword-only and rename `metric` → `metrics`
- **Step 2:** Allow flexible shared sample parameters (e.g., `actions_taken`, `rewards`, `propensities` for contextual bandits)

The goal: support **dataset-only metrics** (like demographic parity that only needs `y_true`), **streaming metrics**, and **metrics from other domains** (contextual bandits, cost-sensitive learning). But the maintainers disagree on whether this flexibility is worth the complexity.

### The Two Sides

**Side A — Generalize MetricFrame (MiroDudik's position):** The current API is "really limiting us." Real use cases exist beyond classification. Keyword-only arguments with optional `y_true`/`y_pred` (`=None`) solve the problem without breaking backward compatibility.

**Side B — Keep MetricFrame simple, build new classes for new use cases (riedgar-ms, hildeweerts):** "I am afraid that trying to fit all different kinds of learning tasks into one MetricFrame will become extremely confusing for novice users." `**kwargs` means you can't add new named arguments later without breaking someone.

### The Compromise That Emerged

After 74 comments over 5 months, the maintainers converged on:
1. ✅ Rename `metric` → `metrics`
2. ✅ Switch to keyword-only arguments
3. ✅ Add `shared_sample_params` as a dictionary (not `**kwargs`)
4. ❌ Optional `y_true`/`y_pred` — still contested
5. ❓ New class for non-standard metrics — deferred

The issue remains **open and unresolved**.

**Discussion prompts:**
- Should a fairness tool be a general-purpose framework or a specialized instrument? What are the trade-offs?
- When does API flexibility become API confusion? Where's the line?
- Who should decide which use cases a fairness tool supports — the maintainers, the users, or the communities being audited?

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