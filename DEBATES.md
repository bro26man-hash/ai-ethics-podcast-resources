# ⚖️ Ongoing Debates in Algorithmic Fairness

A curated summary of real controversies happening in open-source fairness projects — sourced from GitHub issue threads, explainer corrections, and reproduced-bug reports.

---

## Debate 1: The MetricFrame API Design Debate — Who Does the Tool Serve?

**Source:** [fairlearn/fairlearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Tags:** `API`, `design-philosophy`  
**Status:** Open since April 22, 2021 — unresolved, 74 comments, 1 ❤️ reaction  
**Author:** [MiroDudik](https://github.com/MiroDudik) (FairLearn maintainer, issue author)  
**Key participants:** riedgar-ms (co-maintainer), romanlutz (maintainer), hildeweerts (contributor)

### The Core Question

`MetricFrame` is FairLearn's flagship tool for computing fairness metrics with disaggregation by sensitive features. But it only works with metrics that have the signature `metric(y_true, y_pred)`. mainteners MiroDudik proposes two changes:

- **Step 1:** Make all arguments keyword-only and rename `metric` → `metrics`
- **Step 2:** Allow flexible shared sample parameters (e.g., `actions_taken`, `rewards`, `propensities` for contextual bandits)

The goal: support **dataset-only metrics** (like demographic parity that only needs `y_true`), **streaming metrics**, and **metrics from other domains** (contextual bandits, cost-sensitive learning). But the maintainers disagree on whether this flexibility is worth the complexity.

### The Four Scenarios MiroDudik Proposes

1. **Classification + scoring metrics in the same frame:** Evaluate both accuracy and ROC AUC on the same model, with different parameter needs.
2. **Multiple models in the same frame:** Compare three models' accuracy without lambda wrappers for each.
3. **Streaming metrics:** Support incremental data addition for online fairness monitoring.
4. **Metrics beyond classification:** Contextual bandits need `actions_taken`, `rewards`, `propensities` — not `y_true`/`y_pred` at all.

### The Two Sides

**Side A — Generalize MetricFrame (MiroDudik's position):**
- The current API is "really limiting us"
- Real use cases exist beyond classification: dataset-only metrics, streaming, bandits
- Keyword-only arguments with optional `y_true`/`y_pred` (`=None`) solve the problem without breaking backward compatibility
- A transition API with deprecation warnings can ease the change

**Side B — Keep MetricFrame simple, build new classes for new use cases (riedgar-ms, hildeweerts):**
- "I am afraid that trying to fit all different kinds of learning tasks into one MetricFrame will become extremely confusing for novice users"
- "I would not be keen on allowing None for y_true and y_pred; not until we're really sure we want to expand on MetricFrame in this way"
- `**kwargs` in the signature means you can't add new named arguments later without breaking someone
- A dictionary-based `shared_sample_params` is safer than `**kwargs`
- "I think it may be better to take each by itself, and figure out what works best for that domain, rather than trying to shoehorn them into MetricFrame"
- "The majority of users will be looking for classification/regression metrics and may not even be familiar with reinforcement learning"

### The Compromise That Emerged

After 74 comments over 5 months, the maintainers converged on:

1. ✅ **Rename `metric` → `metrics`** (everyone agrees)
2. ✅ **Switch to keyword-only arguments** (everyone agrees)
3. ✅ **Add `shared_sample_params` as a dictionary** (not `**kwargs`) — riedgar-ms's preference
4. ❌ **Optional `y_true`/`y_pred`** — still contested; riedgar-ms says "not keen" until they're sure about expansion
5. ❓**New class for non-standard metrics** — riedgar-ms提议 "a fresh discussion group about what a more general disaggregated metric should look like"

The issue remains **open and unresolved** — the conversation stalled between "make it flexible" and "keep it simple, build separately."

### The Deeper Tension: Who Does the Tool Serve?

This isn't really about Python syntax. It's about a fundamental question in tool design:

**Should a fairness tool serve the use cases its creators imagined, or should it evolve to serve unexpected use cases — even if that makes it harder to learn?**

- **For practitioners** (hiring, lending, lending): Classification metrics are enough. They want a simple, predictable API. They don't need contextual bandits.
- **For researchers** (bandits, causal inference, streaming): The current API is a straitjacket. They need to compute fairness in settings where `y_true` doesn't exist or means something different.
- **For the project's future**: Every generalization makes the API harder to document, harder to test, and harder to extend later. Every specialization fragments the ecosystem.

### Why It Matters Beyond the Repo

1. **The "generalist vs. specialist" tension repeats across every fairness toolkit.** AIF360 tries to be comprehensive (20+ metrics). FairLearn leans toward practical scikit-learn integration. Aequitas focuses on audit-grade reporting. Each choice decides who the tool serves — and who's excluded.

2. **API design is a political act.** Making `y_true` optional means the tool acknowledges that fairness exists in settings beyond classification. Keeping it required means the tool implicitly says: "fairness is a classification problem." That's a philosophical stance disguised as a function signature.

3. **The maintenance bottleneck.** Both FairLearn and AIF360 show the same pattern: maintainers propose changes, contributors offer fixes, but issues stall for years. When the official maintainers are silent, volunteer corrections lack authority — and the "open" in "open source" becomes questionable.

4. **The downstream harm.** If a regulator references FairLearn's MetricFrame documentation, and that documentation doesn't explain what happens when `y_true` is missing, the audit report is incomplete. The API design choice becomes a policy outcome.

**Discussion prompts for the episode:**
- Should a fairness tool be a general-purpose framework or a specialized instrument? What are the trade-offs?
- When does API flexibility become API confusion? Where's the line?
- Who should decide which use cases a fairness tool supports — the maintainers, the users, or the communities being audited?
- Is 74 comments and 5 months without resolution a sign of healthy deliberation or institutional failure?
- If two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?
- Should fairness tool maintainers be required to respond to issues within a certain timeframe? What would that look like?
- Is it ethical to ship a fairness tool whose API implicitly excludes certain use cases (e.g., bandits, streaming)?

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
- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric for intersectional analysis, but the existing metrics still have documentation errors. This raises the question: should you add new metrics before fixing the old ones?
- **[#526 — Memory Management Issue in ClassificationMetric](https://github.com/Trusted-AI/AIF360/issues/526):** Open since April 2024. A core class has performance problems.

The pattern: **documentation errors, infrastructure decay, and performance issues** all coexist. Volunteer corrections are welcome but unassigned.

### Why It Matters Beyond the Repo

1. **The gap between research definitions and production tooling.** Research papers define fairness metrics with mathematical precision. Toolkits wrap them in APIs with docstrings that simplify — and sometimes distort — the original definitions.
2. **Who maintains the definitions?** AIF360 is an IBM Research project. When a volunteer identifies a documentation error and no IBM maintainer responds for 17 months, the question isn't just "who fixes the docstring?" but "who owns the standard?"
3. **The AIF360 vs. FairLearn divergence.** FairLearn (Microsoft, 2,286 stars) provides overlapping metrics with AIF360 but may define or compute them differently. If two mainstream tools disagree on what "average odds difference = 0" means, practitioners and regulators have no single authoritative reference.
4. **The downstream harm.** Courts citing AIF360's metrics, regulators referencing its documentation, and engineers trusting its API — all inherit whatever the docstring says.
5. **The volunteer maintenance trap.** Hanabi9248 offered a concrete fix, but the issue can't be merged without maintainer push.

**Discussion prompts:**
- Should fairness toolkits be required to publish formal verification of their metric definitions — the way cryptographic libraries publish formal proofs?
- Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
- If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
- Is 17 months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?
- When two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?
- Is a volunteer-submitted fix with no maintainer merge pathway actually authoritative?

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

**Community response:** Hanabi9248 (September 2026) volunteered to implement the enhancement — but the issue remains **unassigned and unmerged**.

### Why It Matters

1. **The scalar vs. breakdown tension is a political choice.** A single "fairness score" simplifies regulation but can erase the communities that need the most protection.
2. **Who decides the granularity of fairness reporting?** Regulators, practitioners, or the communities being audited?
3. **The volunteer-maintenance pattern.** Both AIF360 debates (#528 and #558) share the same pattern: a volunteer offers a fix or enhancement, but the issue sits unassigned.

**Discussion prompts:**
- Should fairness tools be required to support intersectional analysis by default, or is a single scalar acceptable for regulatory contexts?
- If a single scalar can mask the worst-off group, is it ethical to ship that as the default output?
- Who should pay for the maintenance of fairness tools that courts and regulators depend on?
- Is a volunteer-submitted PR without maintainer review actually "open source," or is it just a suggestion box?

---

## Debate 4: The Missing-Value Contract — Should Fairness Tools Be Strict or Silent? (FairLearn #1725)

**Source:** [fairlearn/fairlearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Tags:** `bug`, `consistency`, `contract`  
**Status:** Open since September 2026 — maintainer-confirmed fix pending

### The Bug Report

`MetricFrame` raises a `ValueError` when given missing sensitive feature values, while `plot_roc_curve_by_group` **silently drops** the rows with missing values and draws curves for the remaining groups plus an "Overall" curve.

**Minimum reproduction:**
```python
import numpy as np
from sklearn.metrics import accuracy_score
from fairlearn.metrics import MetricFrame, plot_roc_curve_by_group

y_true = np.array([0, 1, 0, 1, 0, 1])
y_pred = np.array([0, 1, 1, 1, 0, 0])
sf = np.array(["a", "a", "b", "b", np.nan, np.nan], dtype=object)

MetricFrame(metrics=accuracy_score, y_true=y_true, y_pred=y_pred, sensitive_features=sf)
# → ValueError: Feature 'sensitive_feature_0' contains missing values.

plot_roc_curve_by_group(y_true=y_true, y_score=y_score, sensitive_features=sf)
# → draws Overall, "a", "b"; the two NaN rows are silently dropped
```

### The Two Sides

**Side A —.Make the plot match MetricFrame (strict consistency).** Contributor LobsterQBA argued that `plot_roc_curve_by_group` should reject missing values just like `MetricFrame` does.

**Side B — Keep the skip behavior and fix the docs (permissive pragmatic).** Dropping rows might be the right behavior for visualization.

**Maintainer's verdict (romanlutz, Sept 16):** "Missing sensitive feature values should be rejected with a clear `ValueError` in both APIs, consistent with MetricFrame after #1698."

### Why It Matters

1. **Should fairness tools be strict or permissive by default?** A strict `ValueError` protects users from invisible bias, but it also blocks legitimate analyses where missing data is the norm.
2. **The "Overall vs. Groups" trap.** When a tool silently drops rows, the "Overall" curve includes everyone while group curves exclude some — creating a comparison that doesn't add up.
3. **The contract question.** What should the *documented contract* of a fairness function be?

**Discussion prompts:**
- Should fairness tools raise errors on missing sensitive data, or should they handle it gracefully?
- Is silent row-dropping a bug or a feature? When would you want to skip missing data vs. reject it?
- If two functions in the same toolkit have different missing-data behaviors, is that a bug — or a design choice?

---

## Debate 5: The SHAP Measurement Problem — Does the Denominator Change the Story? (Fair-Code #672)

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

## Debate 6: The Impossibility Triangle — Which Fairness Metric Wins?

**Source:** Fair-Code Explainer + [Issue #665](https://github.com/yakew7/Fair-Code/issues/665)  
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)  
**Tags:** `documentation`, `bug`  
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