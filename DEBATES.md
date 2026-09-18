# ⚖️ Ongoing Debates in Fairness Tooling — From Real Issue Threads

These summaries are drawn from live, unresolved issue threads on the repositories tracked in [RESOURCES.md](./RESOURCES.md). They capture the genuine tensions that shape how fairness tools are built — and who they end up serving.

---

## Debate 1: Who Should a Fairness Tool Serve? — FairLearn #756

**Issue:** [`MetricFrame should support metrics that don't require y_true and y_pred`](https://github.com/fairlearn/fairlearn/issues/756)
**Repo:** [fairlearn/fairlearn](https://github.com/fairlearn/fairlearn)
**Opened:** April 2021 · **Status:** Still open · **Comments:** 74
**Labels:** `API`

### The Core Tension

Fairlearn's `MetricFrame` is its centerpiece API for evaluating model fairness across demographic groups. But it was designed for a specific pattern: you give it `y_true` (ground truth) and `y_pred` (predictions), and it computes metrics like accuracy, precision, or demographic parity disaggregated by sensitive features.

Researcher Miro Dudik (Fairlearn maintainer) argued that this design is **too limiting** for real-world use cases:

1. **Dataset-only metrics** (e.g., demographic parity, which needs only `y_true` — no predictions)
2. **Metrics beyond classification** (e.g., contextual bandit metrics that need `actions`, `rewards`, `propensities`)
3. **Streaming metrics** where data arrives incrementally and `y_true`/`y_pred` are empty at initialization

Dudik proposed making `y_true` and `y_pred` optional and adding a `shared_sample_params` dictionary so metrics with different signatures could share sample-level data.

### The Counterargument: Keep It Simple
 **riedgar-ms** (Fairlearn maintainer) pushed back hard:

- Making `y_true`/`y_pred` optional would **break existing code** and force a major version bump
- `**kwargs` (keyword arguments) in the API signature would make it **impossible to ever add new named arguments** without breaking someone again
- `MetricFrame` does a "fairly simple thing well" — maybe it should stay that way, and new problem domains should get their own classes
- The API should stay **intuitive for novice users** who aren't familiar with reinforcement learning or bandit settings

**hildeweerts** agreed: "The majority of users will be looking for classification/regression metrics and may not even be familiar with reinforcement learning. Having to look at examples to understand how to use an API even in the 'simplest' scenario, signals that it is not intuitive."

### The Middle Ground That Emerged

After 74 comments and multiple iterations, the maintainers converged on:

- **Step 1:** Rename `metric` → `metrics` and switch to keyword-only arguments (breaking change, but manageable)
- **Step 2:** Add `shared_sample_params` as a **dictionary** (not `**kwargs`) to pass extra sample data to metrics
- Key design decision: `y_true` and `y_pred` should remain **required** (not optional) to preserve the API's clarity

### 🎙️ Podcast Talking Points

- **The "who is this for?" question is never just technical.** Every API decision — what's required, what's optional, how you pass data — encodes an assumption about the user. Fairlearn's maintainers explicitly discussed whether to optimize for novice practitioners or researchers pushing boundaries.
- **Backward compatibility as a value.** The maintainers were deeply concerned about breaking existing code. This isn't just pragmatism — it's a philosophical stance about who has already adopted the tool and what their investment means.
- **Simplicity vs. flexibility is a values debate, not a technical one.** The maintainers didn't disagree about whether flexibility was "good." They disagreed about *whose* flexibility mattered more — and what the cost of complexity would be for the next person who reads the code.
- **The "dummy column" workaround.** One maintainer suggested users could just pass dummy `y_true` values to work around the limitation. This offhand suggestion reveals a power dynamic: the person suggesting it didn't bear the burden of the hack.

---

## Debate 2: When the Metric Itself Is Wrong — AIF360 #528

**Issue:** [`average_odds_difference` metric is wrongly represented as an equalized odds relaxation](https://github.com/Trusted-AI/AIF360/issues/528)
**Repo:** [Trusted-AI/AIF360](https://github.com/Trusted-AI/AIF360)
**Opened:** April 2024 · **Status:** Still open · **Comments:** 1 (but 2 👍 reactions)

### The Problem

AndreFCruz filed a bug report: the `average_odds_difference` metric in AIF360 is documented as "A value of 0 indicates equality of odds." But that's **mathematically incorrect**. Any pair of points on a specific line would yield `average_odds_difference = 0` without actually fulfilling the equalized odds criterion.

This isn't just a documentation typo. If a practitioner relies on this metric to audit a hiring model for equalized odds, they could falsely conclude the model is fair when it isn't. The error has propagated to IBM's official fairness documentation website as well.

### 🎙️ Podcast Talking Points

- **Trust is the real currency of fairness tools.** When a metric is named after a fairness concept ("equalized odds"), users assume it measures that concept. A mismatch between name and mathematical definition isn't a bug — it's an epistemological risk.
- **Who audits the auditors?** AIF360 is used by regulators and companies to claim compliance with fairness standards. If the metric definitions inside the tool are wrong, the compliance claims built on them are void.
- **The gap between research and deployment.** This issue was filed in 2024 and remains open. The toolkit that bridges academic fairness research and real-world deployment has open questions about its own correctness.

---

## Debate 3: Intersectional Auditing — AIF360 #558

**Issue:** [`Extend Empirical Differential Fairness metric`](https://github.com/Trusted-AI/AIF360/issues/558)
**Repo:** [Trusted-AI/AIF360](https://github.com/Trusted-AI/AIF360)
**Opened:** January 2026 · **Status:** Still open · **Comments:** 1

### The Issue

The current Empirical Differential Privacy (EDF) metric in AIF360 returns a **single scalar** summarizing fairness across all protected attribute combinations. This makes it impossible to identify *which specific group pairs* drive the maximum log-ratio — the whole point of an intersectional analysis.

jetverbeek requested that the metric return:
- The pair(s) of attribute groups that produce the maximum log ratio
- A breakdown of all (or top-X) group combinations and their respective log-ratios

A contributor offered to implement it, but the issue remains unassigned and unresolved.

### 🎙️ Podcast Talking Points

- **A scalar can hide harm.** Reducing intersectional disparity to a single number makes it possible for a model to be "fair" on average while being severely biased against specific subgroup combinations (e.g., Black women, disabled immigrants).
- **Contribution vs. maintenance gap.** A community member offered to implement intersectional breakdowns — but nobody was assigned to review or merge the work. This pattern (community contribution, no maintainer bandwidth) is common in fairness tooling.
- **The "who decides what matters" problem.** The current EDF implementation prioritizes a summary statistic. The requested change prioritizes granular, actionable audit data. Both are valid — but they serve different audiences and different decisions.

---

## 🔑 Cross-Cutting Themes

| Theme | FairLearn #756 | AIF360 #528 | AIF360 #558 |
|-------|----------------|-------------|-------------|
| Who does this tool serve? | Practitioners vs. researchers | Audit consumers | Audit consumers |
| Simplicity vs. power | Core design tension | N/A | Scalar vs. granular |
| Backward compatibility | Central concern | N/A | N/A |
| Trust & correctness | N/A | Metric definition is wrong | Aggregation hides harm |
| Maintainer bandwidth | Active debate, 74 comments | Open since 2024 | Open since 2026 |

---

## 🤝 How to Contribute

1. **Found a debate in another issue thread?** Add it to this file with a link and a brief summary
2. **Want to argue a different side?** Open a discussion issue in this repo — our [discussion prompt](./issues) is a great starting point
3. **Want来分析 these debates further?** Check out the original issue threads and join the conversation
