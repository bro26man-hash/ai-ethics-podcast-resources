# ⚖️ Ongoing Debates in Algorithmic Fairness

A curated summary of real controversies happening in open-source fairness projects — sourced from GitHub issue threads, explainer corrections, and reproduced-bug reports.

---

## Debate 1: The SHAP Measurement Problem — Does the Denominator Change the Story?

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

## Debate 2: The Counterfactual Fairness Reversal — When the Reproduction Contradicts the Argument

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

## Debate 3: The Impossibility Triangle — Which Fairness Metric Wins?

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

## Debate 4: The Average-Odds Mislabeling — When the Metric Definition Itself Is the Debate

**Source:** [IBM AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Project:** [IBM AIF360 (AI Fairness 360)](https://github.com/Trusted-AI/AIF360)  
**Tags:** `documentation`, `bug`  
**Status:** Open since April 2024 — unresolved for over 2 years, 2 👍 reactions

### The Claim (Side A: Morgan) On   

"The average-odds metric is listed as 'A value of 0 indicates equality of odds.'"

The IBM AIF360 documentation and tutorial define `average_odds_difference` as a metric where 0 = equalized odds satisfied. This claim appears in:
- The official AIF360 Python API documentation (`aif360.metrics.ClassificationMetric.average_odds_difference`)
- IBM's online fairness metrics tutorial (dataplatform.cloud.ibm.com)
- Countless downstream tutorials, blog posts, and academic papers that cite AIF360 as the reference

### The Refutation (Side B: AndreFCruz)  

"This doesn't seem to be true, as any pair of points on the following line would have `average_odds_difference=0` but not fulfill equalized odds necessarily."

AndreFCruz provides a visual proof (screenshot in the issue) showing that the line where `average_odds_difference = 0` is **not** the same as the line where equalized odds holds. Two ROC curves can intersect at a point where the average odds difference is zero, yet the false positive rates and true positive rates differ across groups — meaning equalized odds is violated.

**The core technical argument:** `average_odds_difference` is mathematically distinct from `average_abs_odds_difference` and from `equalized_odds_difference`. Conflating them in documentation doesn't just confuse beginners — it means that anyone using AIF360's documentation as a reference could systematically misdiagnose whether a model satisfies equalized odds.

### Why This Is a Podcast-Grade Debate (Not Just a Doc Bug)

1. **The error has been open for 2+ years.** Filed in April 2024, still open in September 2026. The metric definition has been cited in IBM's public tutorials and presumably in many downstream works during that time.

2. **The stakes are real, not academic.** If a court uses "average odds difference = 0" as evidence of equalized odds, but the metric doesn't actually measure equalized odds, the legal and policy consequences are concrete — wrongful denial of benefits, sentencing, or loans based on a false fairness signal.

3. **Documentation as de facto standard.** AIF360 is the most widely used fairness toolkit in both industry and research. When its documentation says one thing, the ecosystem believes it. A mislabeling doesn't just affect one repo — it propagates through every paper, lecture, and policy document that cites it.

4. **The fix is concrete but the conversation is deeper.** The latest comment (Sept 2026) from Hanabi9248 offers to fix both the API docstring and the MetricTextExplainer output with a four-row example where `average_odds_difference=0` while `average_abs_odds_difference=1` and `equalized_odds_difference=1`. But the deeper question remains: **how do fairness metric definitions get standardized, and who holds the authority to correct them?**

### The Deeper Question   

This debate surfaces a structural problem in the fairness ecosystem:

| Question | Why It Matters |
|---|---|
| Who gets to define what "fairness" means in a machine? | The toolkit author's documentation choice becomes the community's definition |
| Can a metric name create a false sense of precision? | "Average odds difference" sounds like it measures odds equality — but it doesn't |
| What's the remedy when a widely-used tool has a documentation error? | Open an issue? Submit a PR? Or is the entire metric library flawed from a conceptual level? |
| Should fairness metrics be audited the way security vulnerabilities are? | CVEs for fairness — a formal process for identifying and correcting definitional errors |

**Discussion prompts for the episode:**
- If the most popular fairness toolkit mislabels its own metric for 2+ years, should we trust ANY fairness metric?
- Is the fix a docstring change, or does it reveal a deeper conceptual confusion between similar-sounding fairness definitions?
- Should there be a "fairness CVE" process — a formal way to flag and correct definitional errors in widely-cited tools?
- Who bears the cost when documentation errors propagate into policy: the tool maintainers, the organizations that deploy the tools, or the communities subjected to the biased decisions?

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