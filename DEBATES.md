# ⚡ Ongoing Debates in AI Fairness

A living document summarizing real controversies from open-source fairness toolkits — perfect material for podcast deep-dives.

---

## 🔥 Debate #1: How Should Fairness Metrics Handle Non-Standard Data?

**Source:** [fairlearn/fairlearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756)
**Labels:** `API` | **Status:** Open (74 comments, no resolution)
**Date opened:** April 2021 | **Last updated:** September 2021 (still open!)

### The Core Question

Microsoft's Fairlearn toolkit measures fairness by comparing model predictions across demographic groups. Its central tool, `MetricFrame`, was designed for standard classification/regression metrics that take `(y_true, y_pred)` as arguments. But real-world fairness evaluation often needs metrics that *don't* fit that pattern — for example:

- **Contextual bandit fairness**: In lending, you only observe repayment (reward) for the loan type you offered (action), not for counterfactual alternatives.
- **Cost-sensitive learning**: The metric depends on cost matrices, not predicted labels.
- **Streaming metrics**: Data arrives continuously; you don't have a fixed dataset to score against.
- **Dataset-only metrics**: Metrics like demographic parity only need the dataset, not a model's predictions.

The question: **Should Fairlearn's `MetricFrame` be redesigned to support these non-standard metrics, or should separate tools be built for different problem domains?**

### The Two Positions

#### 🟢 Position A: Expand MetricFrame (MiroDudik, Fairlearn maintainer)

- Make `y_true` and `y_pred` *optional* keyword arguments (defaulting to `None`).
- This lets users pass any metric signature, from classification to bandits, without "dummy" arguments.
- **Key quote:** *"The current API is really limiting us, so I'd rather fix this sooner rather than commit to the future of docs that describe all kinds of workarounds."*
- **Underlying philosophy:** A fairness tool should be a *universal framework* for evaluating any ML system, regardless of problem type. Flexibility matters more than simplicity.

#### 🔴 Position B: Keep MetricFrame Focused (riedgar-ms, hildeweerts, other maintainers)

- `y_true` and `y_pred` should remain **required** arguments. Making them optional opens new failure modes and dilutes the tool's clarity.
- `**kwargs` (the proposed way to pass arbitrary parameters) is dangerous: any future parameter name could collide with a user's keyword, creating subtle bugs.
- Different problem types (reinforcement learning, NLP, vision) have fundamentally different metric needs — they deserve *separate classes*, not a bloated generalist.
- **Key quote (hildeweerts):** *"The majority of users will be looking for classification/regression metrics and may not even be familiar with reinforcement learning. Having to look at examples to understand how to use an API even in the 'simplest' scenario signals that it is not intuitive."*
- **Underlying philosophy:** A fairness tool should do *one thing well* and stay accessible to non-experts. "Capable of everything" often means "confusing for everyone."

### Where It Ended Up

The community eventually converged on a **compromise ("Alternative A")**:
- `y_true` and `y_pred` become *optional* (keyword-only, defaulting to `None`).
- No `**kwargs` — a `sample_params` dictionary still handles extra arguments.
- This covers streaming, bandit, and cost-sensitive scenarios without full `**kwargs` flexibility.

**But the issue remains open.** The philosophical tension — between generality and clarity, between power users and novices — has not been resolved. As of the latest update, this debate has been running for **over three years**.

### 🎙️ Podcast Talking Points

1. **Who does a fairness tool serve?** The maintainer argues for liberty-loving data scientists who need flexibility; his colleagues argue for the typical user who just wants a simple, predictable API.
2. **Can "fairness" be measured a single way?** The existence of 10+ competing fairness metrics (demographic parity, equalized odds, predictive parity…) suggests there's no neutral answer — each metric encodes a different theory of justice.
3. **Does tool design reflect power dynamics?** When a maintainer pushes for `**kwargs` and others resist, is that a technical debate — or a power struggle over who gets to define what "fairness" means?
4. **The long tail of open issues:** This issue has been open since 2021. What does it mean that the fairness community spends years debating API design while real-world biased algorithms keep running?

---

## 🔥 Debate #2: What Do Fairness Metrics Actually Measure? The Utility-vs-Fairness War Inside AIF360

**Source:** [Trusted-AI/AIF360 Issue #214](https://github.com/Trusted-AI/AIF360/pull/214) (and extended discussion in comments)
**Labels:** `metrics` | **Status:** Open (9 comments, over 3 years)
**Date opened:** November 2020 | **Last updated:** February 2023

### The Core Question

AIF360 categorizes several "inequality indices" (like the Generalized Entropy Index, GEI) as *individual fairness metrics*. But a contributor named **leenamurgai** made a startling claim: **these indices don't measure fairness at all — they measure utility.** This isn't a refactoring suggestion; it's a fundamental challenge to how the toolkit classifies its own measurements.

### The Three Positions

#### 🟢 leenamurgai — "These are utility metrics disguised as fairness metrics"

- Submitted a long mathematical proof showing that GEI can be rewritten as a function of two parameters: **model accuracy (λ)** and **mean benefit (μ)** — both utility concepts, not fairness concepts.
- Proved that minimizing GEI is equivalent to minimizing cross-entropy loss (when α=0) or mean squared error (when α=2) — both standard ML optimization objectives, not fairness objectives.
- Argued that the two "individual fairness" definitions in the literature are **contradictory at a conceptual level**: Dwork et al. (2011) defines individual fairness as a property of a *single map* (ground truth provided by a similarity metric), while Speicher et al. (2018) defines it via distributional comparisons. You can't classify both as "individual fairness."
- **Key quote:** *"It is misleading to have GEE categorised under individual fairness metrics alongside consistency. Individual fairness as defined by Dwork et al. is importantly not a measure of utility."*
- Concluded that the famous "fairness-utility trade-off" paper isn't showing a trade-off between group fairness and individual fairness at all — it's just showing the well-known utility-vs-fairness trade-off.
- **Political implication:** If we miscategorize utility as fairness, we risk *selling* utility optimization to policymakers as if it were fairness enforcement. "Choosing a benefit function is choosing whose interests count as 'beneficial' — and that's a political decision, not a technical one."

#### 🟡 hoffmansc — "They measure a different kind of fairness — let's call it 'distributional fairness'"

- Agreed that GEI doesn't match Dwork et al.'s definition of individual fairness.
- But pushed back on the claim that it's *only* utility: GEI has no dependence on the feature space X (it's "anonymous" — only looks at predictions and ground truth), which is similar to utility functions but could still measure a *distributional* property.
- Proposed reclassifying these indices as **"distributional fairness"** metrics — a separate category from both individual fairness and group fairness.
- Asked practical questions: Should we calculate overall GEI or only between-group? What about within-group? These questions have real implications for how auditors interpret results.
- **Key quote:** *"Perhaps we could categorize them separately as, say, 'distributional fairness'?"*
- Showed willingness to accept leenamurgai's findings and update documentation, but wanted careful treatment.

#### 🔴 The Implicit Position — "This doesn't matter in practice"

- The debate has received **9 comments over 3 years** and remains unresolved. Several core maintainers of AIF360 (including Kurt Varshney, Michael Hind) have not weighed in on the mathematical substance.
- The metrics remain categorized as "individual fairness" in the documentation and sklearn API.
- **What this silence means:** Either the maintainers haven't engaged with the critique, or they consider it too theoretical for practical impact. Both possibilities are concerning for a toolkit used by real auditors and policymakers.

### Why This Debate Matters for Social Justice

This isn't an abstract math dispute. It has real consequences:

1. **Misleading governance:** If AIF360 labels a utility metric as "individual fairness," an auditor using the toolkit might believe they are enforcing a rigorous fairness standard when they are merely optimizing model performance. This could lead to false certification of biased systems as "fair."

2. **Whose interests count?** The benefit function at the heart of GEI encodes *whose outcomes count as beneficial*. When leenamurgai asks "is it meaningful to ignore people if you're trying to be fair?", he's asking whether fairness metrics should consider *everyone* affected — or only the groups explicitly designated as sensitive attributes. If a welfare algorithm's benefit function only looks at qualification rates, it may miss the community that considers *being considered human* the baseline benefit.

3. **The language problem:** The debate about whether to call these metrics "fairness" or "utility" echoes the broader argument in issue #97 about whether a tool can even "remove bias." If we can't agree on what our tools measure, how can we trust what they tell us?

### 🎙️ Podcast Talking Points

1. **The marketing problem:** Fairness toolkits sell "fairness" to companies and governments. If the metrics inside are really measuring utility, the product is fundamentally misrepresented. Who bears the cost of that misrepresentation?
2. **The(约) "Who is the customer?" question:** Fairlearn maintainers debate whether MetricFrame should serve data scientists or the communities affected by models. AIF360 maintainers haven't weighed in on whether their metrics serve auditors or utility optimizers. Are these the same question?
3. **The silence is also a position:** When senior maintainers don't respond to a 3-year-old mathematical critique, what message does that send about the community's commitment to intellectual honesty?
4. **The Dwork vs. Speicher paradox:** Two foundational papers define "individual fairness" in contradictory ways. Both are widely cited. Neither Wikipedia nor the textbooks reconcile them. How did the field get here?

---

## 📝 Contributing Debates

Have you found another heated thread in a fairness repo? Add it here!

| Issue | Repo | Core Question |
|---|---|---|
| [fairlearn #756](https://github.com/fairlearn/fairlearn/issues/756) | fairlearn/fairlearn | Should `MetricFrame` support metrics without `y_true`/`y_pred`? |
| [AIF360 #214](https://github.com/Trusted-AI/AIF360/pull/214) | Trusted-AI/AIF360 | Do "inequality indices" measure fairness or utility? |
| [AIF360 #97](https://github.com/Trusted-AI/AIF360/issues/97) | Trusted-AI/AIF360 | Can a tool "remove bias"? Should we reconsider the word "bias"? |

To add a new debate: fork this repo, append to `DEBATES.md`, and open a PR!
