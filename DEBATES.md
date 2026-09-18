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

## Debate 2: The Missing-Value Contract — Should Fairness Tools Be Strict or Silent?

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

## Debate 3: The Audit-Grade vs. Research-Grade Divide — Who Is Fairness Tools Built For?

**Source:** [Aequitas Issues #201 & #86](https://github.com/dssg/aequitas/issues/201) + [AIF360 #548](https://github.com/Trusted-AI/AIF360/issues/548)  
**Projects:** [Aequitas — DSG](https://github.com/dssg/aequitas), [AIF360 — IBM](https://github.com/Trusted-AI/AIF360)  
**Tags:** `documentation`, `maintenance`  
**Status:** Ongoing

### The Tension

Aequitas was built for **audit contexts** — end-to-end fairness auditing with documentation and reporting. Its design assumes the auditor is a *practitioner* who needs to produce evidence. But [Issue #201](https://github.com/dssg/aequitas/issues/201) reveals that even its own documentation has gaps: "Add a readme or page on the existing metrics of fairness" — an open issue for a toolkit whose entire purpose is making fairness measurable.

Meanwhile, AIF360's [website has been down](https://github.com/Trusted-AI/AIF360/issues/548) since February 2025, and its `average_odds_difference` docstring has been wrong since April 2024.

### The Two Sides

**Side A — Evidence-grade tools must prioritize documentation.** If a tool claims to help auditors produce court-ready evidence, it must ship with complete, accurate documentation. A missing website and a misleading docstring undermine the tool's core purpose.

**Side B — Tools should prioritize functionality over documentation.** In fast-moving research projects, documentation lags behind code. The metrics work even if the docs don't perfectly describe them. Practitioners can consult the source code or the original research papers.

### Why It Matters

The audit-grade vs. research-grade divide is really about **who fairness tools serve**:

| Priority | Built For | Risk |
|---|---|---|
| **Evidence-grade** | Auditors, regulators, courts | Stale docs, slow iteration |
| **Research-grade** | ML researchers, paper authors | Unverified metrics, poor discoverability |

When a tool is used in a court case, does the maintainer have a duty to ensure the documentation is correct? When a volunteer finds a docstring error and the maintainer is silent for 17 months, who is accountable?

**Discussion prompts for the episode:**
- Should fairness tools be classified as "medical device" or "court evidence" — with corresponding documentation requirements?
- Who is liable when a documentation error in a fairness toolkit leads to a biased decision in a court?
- Can a tool be both evidence-grade and research-grade, or do those goals conflict?
- Should maintainers of fairness toolkits be required to respond to documentation issues within a certain timeframe?

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