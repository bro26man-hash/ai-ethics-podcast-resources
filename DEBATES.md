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

### Why It Matters Beyond the Repo

This is not a niche documentation cleanup. It reveals a structural tension in the fairness toolkit ecosystem:

1. **The gap between research definitions and production tooling.** Research papers define fairness metrics with mathematical precision. Toolkits wrap them in APIs with docstrings that simplify — and sometimes distort — the original definitions.

2. **Who maintains the definitions?** AIF360 is an IBM Research project. When a volunteer identifies a documentation error and no IBM maintainer responds for 17 months, the question isn't just "who fixes the docstring?" but "who owns the standard?"

3. **The AIF360 vs. FairLearn divergence.** FairLearn (Microsoft, 2,286 stars) provides overlapping metrics with AIF360 but may define or compute them differently. If two mainstream tools disagree on what "average odds difference = 0" means, practitioners and regulators have no single authoritative reference.

4. **The downstream harm.** Courts citing AIF360's metrics, regulators referencing its documentation, and engineers trusting its API — all inherit whatever the docstring says. A misleading docstring isn't an academic issue; it's a real-world harm vector.

**Discussion prompts for the episode:**
- Should fairness toolkits be required to publish formal verification of their metric definitions — the way cryptographic libraries publish formal proofs?
- Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
- If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
- Is 17 months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?
- When two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?

---

## Debate 2: The SHAP Measurement Problem — Does the Denominator Change the Story?

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

## Debate 3: The Counterfactual Fairness Reversal — When the Reproduction Contradicts the Argument

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

## Debate 4: The Impossibility Triangle — Which Fairness Metric Wins?

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