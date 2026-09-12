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
- **Key quote:** *"The current API is really limiting us, so I'd rather fix this sooner rather than commit to the future of docs that describe all kinds of workarounds."\*
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

## 📝 Contributing Debates

Have you found another heated thread in a fairness repo? Add it here!

| Issue | Repo | Core Question |
|---|---|---|
| [fairlearn #756](https://github.com/fairlearn/fairlearn/issues/756) | fairlearn/fairlearn | Should `MetricFrame` support metrics without `y_true`/`y_pred`? |

To add a new debate: fork this repo, append to `DEBATES.md`, and open a PR!
