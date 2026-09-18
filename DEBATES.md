# ⚖️ Ongoing Debates in Algorithmic Fairness

A curated summary of real controversies happening in open-source fairness projects — sourced from GitHub issue threads, explainer corrections, and reproduced-bug reports.

---

## Debate 1: The MetricFrame API Design Debate — Who Does the Tool Serve?

**Source:** [fairlearn/fairlearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756)
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)
**Tags:** `API`, `design-philosophy`
**Status:** Open since April 22, 2021 — unresolved (over 5 years), 74 comments
**Author:** [MiroDudik](https://github.com/MiroDudik) (FairLearn maintainer, MIC member)

### The Core Question

`MetricFrame` is FairLearn's flagship tool for computing fairness metrics with disaggregation by sensitive features. But it only works with metrics that have the signature `metric(y_true, y_pred)`. Maintainer MiroDudik proposes two changes that would fundamentally reshape the tool:

- **Step 1:** Make all arguments keyword-only and rename `metric` → `metrics`
- **Step 2:** Allow flexible shared sample parameters (e.g., `actions_taken`, `rewards`, `propensities` for contextual bandits)

The goal: support **dataset-only metrics**, **streaming metrics**, and **metrics from other domains** (cost-sensitive learning, reinforcement learning). But the maintainers disagree on whether this flexibility is worth the complexity.

### The Two Sides — In Their Own Words

**Side A — Generalize MetricFrame (MiroDudik's position):**

> "The current API is really limiting us." *(Comment 3)*

MiroDudik argues that real use cases exist beyond classification:
- Dataset-only metrics (e.g., demographic parity needs only `sensitive_features`, no `y_true`/`y_pred`)
- Streaming metrics (data arrives incrementally)
- Contextual bandit metrics (parameters are `actions_taken`, `rewards`, `propensities`)
- Cost-sensitive learning (parameters are `costs`, `y_pred`)

He proposes **Alternative A**: make `y_true` and `y_pred` optional (default `None`), with all parameters passed as keyword arguments:

```python
# Proposed new API
MetricFrame(*, metrics, y_true=None, y_pred=None, sensitive_features=None, 
            control_features=None, sample_params=None)
```

He demonstrates with concrete code — for example, replacing confusing wrapper lambdas with clean direct calls:

```python
# Before: confusing wrapper for a metric that doesn't need y_true
def ips_wrapper(y_true, y_pred, *, actions_taken, rewards, propensities, actions_target):
    return inverse_propensity_score(actions_taken=actions_taken, rewards=rewards,
                                    propensities=propensities, actions_target=actions_target)

mf = MetricFrame(ips_wrapper, dummy_y_true, dummy_y_pred, sensitive_features=sf,
                 sample_params={'actions_taken': actions_taken, ...})

# After: clean, direct usage
mf = MetricFrame(metrics=inverse_propensity_score,
                 sample_params={'actions_taken': actions_taken, 'rewards': rewards,
                                'propensities': propensities, 'actions_target': actions_target})
```

He also proposes **Alternative B** with `**direct_sample_params` using `inspect.signature()` to automatically route arguments — but acknowledges it introduces "magic."

**Side B — Keep MetricFrame simple, build new classes for new use cases (riedgar-ms, hildeweerts):**

> "I am afraid that trying to fit all different kinds of learning tasks into one MetricFrame will become extremely confusing for novice users." — *hildeweerts (Comment 5)*

> "`**kwargs` in the signature means that if we ever want to add new arguments to the signature, we're going to break someone." — *riedgar-ms (Comment 25)*

> "The majority of users will be looking for classification/regression metrics and may not even be familiar with reinforcement learning." — *hildeweerts (Comment 4)*\n
riedgar-ms's core argument: **MetricFrame does a fairly simple thing well** (taking an sklearn-style metric and turning it into one with grouping). He'd rather keep it that way and work out the best API for new use cases from a clean sheet:

> "I would probably be better to keep the existing MetricFrame API around, doing its simple job." *(Comment 12)*

He proposes `shared_sample_params` as a dictionary parameter (not `**kwargs`):

> "We 'explode' the `Dict[str, vector]` when the metric functions themselves are invoked." *(Comment 9)*

### The Compromise That Emerged After 74 Comments Over 5 Months

1. ✅ **Rename `metric` → `metrics`** (everyone agrees)
2. ✅ **Switch to keyword-only arguments** (everyone agrees)
3. ✅ **Add `shared_sample_params` as a dictionary** (not `**kwargs`) (everyone agrees)
4. ❌ **Optional `y_true`/`y_pred`** — still contested. riedgar-ms: "I would not be overjoyed at defaulting `y_true` and `y_pred` to `None`. That is not how sklearn metrics work."
5. ❓**New class for non-standard metrics** — still open. riedgar-ms: "I would like to know more before deciding to support those by extending MetricFrame (rather than creating a new class)."

### The Deeper Tension: Who Does the Tool Serve?

This isn't just an API design debate. It's a question about the **politics of Fairness tooling**:

**For practitioners** (hiring, lending, compliance): Classification metrics are enough. They want a simple, predictable API that works like sklearn. They don't need contextual bandits or streaming metrics.

**For researchers** (bandits, causal inference, streaming, RL): The current API is a straitjacket. They need to audit fairness in novel settings where `y_true` and `y_pred` don't exist.

**For the project's future**: Every generalization makes the API harder to document, test, and extend. Every specialization fragments the ecosystem. The "right" answer depends on who you believe the tool is *for*.

**The unspoken question**: When a fairness tool is designed by and for Microsoft's MLOps ecosystem, does it inherently prioritize the needs of large-scale deployers over the needs of researchers working on Cutting-edge fairness problems? And if so, whose fairness gets measured?

### Why This Matters Beyond the Repo

1. **API design is a values statement.** Choosing to support only `metric(y_true, y_pred)` means choosing to measure fairness only in classification/regression contexts. It silently excludes entire domains of ML where bias can occur — bandits, recommendation systems, generative models.
2. **The novice-expert tradeoff.** hildeweerts's concern about novice users is legitimate — but it's also a form of gatekeeping. If the tool is too simple, it can't capture the complexity of real-world fairness problems.
3. **The fragmentation risk.** If FairLearn creates separate classes for each problem type, we get `SupervisedMetricFrame`, `UnsupervisedMetricFrame`, `ReinforcementMetricFrame` — and practitioners have to know which one to use before they even understand the fairness problem.
4. **The maintainer power dynamic.** MiroDudik (MIC member, issue author) has push for generalization. riedgar-ms has push for simplicity. The community is split. There's no clear "user" voice in this thread — only maintainer perspectives.
5. **The intersection with regulation.** EU AI Act and NIST AI RMF require fairness assessments across diverse use cases. If FairLearn's API can't handle those use cases, regulators may be forced to rely on tools that are either too narrow or too complex.

**Discussion prompts for the episode:**
- Should a fairness tool be a general-purpose framework or a specialized instrument? Where's the line?
- When does API flexibility become API confusion? Who decides where that line is?
- Who should decide which use cases a fairness tool supports — the maintainers, the users, or the communities being audited?
- Is the "keep it simple" argument a legitimate usability concern, or is it a way of avoiding the hard work of supporting novel fairness problems?
- If a fairness tool can't measure bias in recommendation systems or generative models, is it a failure of the tool — or a limitation that practitioners should work around?
- Should FairLearn have a "community advisory board" to represent the perspectives of non-maintainer users?

---

## Debate 2: The Average-Odds Documentation Bug — What Does "Zero" Mean in AIF360?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)
**Project:** [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360)
**Tags:** `documentation`, `bug`
**Status:** Open since April 23, 2024 — unresolved (2+ years), 2 👍 reactions, 1 correction comment (September 11, 2026)
**Issue author:** [AndreFCruz](https://github.com/AndreFCruz)
**Volunteer:** [Hanabi9248](https://github.com/Hanabi9248) (comment, Sept 2026)

### The Bug Report

AndreFCruz filed this issue after noticing that the [AIF360 documentation](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> "A value of 0 indicates equality of odds."

for the `average_odds_difference` metric.

But that's mathematically incorrect. As the issue's attached diagram shows, there are configurations where `average_odds_difference = 0` yet **equality of odds does not hold**. The metric formula and the equalized-odds criterion are measuring different things, and the documentation conflates them.

The same error appears on IBM's own fairness metrics explainer page.

### The Two Sides

**Side A — This is a documentation bug, not a conceptual one.** The metric itself is well-defined; the problem is that the docstring says "equality of odds" when it should say something like "a relaxation of equality of odds" or "average odds difference." Fix the words, not the math.

**Side B — The terminology matters because it shapes policy.** If AIF360 — the most widely used fairness toolkit in production and government — labels a metric as indicating "equality of odds" when it doesn't, then every audit report, regulatory filing, and court brief that cites this metric inherits the error. The documentation isn't just describing the tool; it's defining what "fairness" means in practice.

### The Community Response

After **17+ months** with no maintainer response, Hanabi9248 volunteered in September 2026 with a focused correction:

> "Could you assign this to me? The cancellation also appears in MetricTextExplainer and its JSON output. I have a focused correction for both API docstrings and the explanations, with a four-row example where average_odds_difference is 0 while average_abs_odds_difference and equalized_odds_difference are both 1. The metric formulas remain unchanged."

But the issue remains open, unassigned, and unmerged.

### The Broader AIF360 Maintenance Picture

This documentation bug is not an isolated case. During research, we found several other open issues in the AIF360 repo that paint a picture of a project under strain:

- **[#548 — Website is down](https://github.com/Trusted-AI/AIF360/issues/548):** Open since February 2025 with 7 comments. The official documentation website has been unavailable, making it harder for users to access metric definitions.
- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric for intersectional analysis, but the existing metrics still have documentation errors.
- **[#526 — Memory Management Issue in ClassificationMetric](https://github.com/Trusted-AI/AIF360/issues/526):** Open since April 2024. A core class has performance problems.

The pattern: **documentation errors, infrastructure decay, and performance issues** all coexist. Volunteer corrections are welcome but unassigned.

**Discussion prompts:**
- Should fairness toolkits be required to publish formal verification of their metric definitions?
- Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
- If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
- Is 17+ months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?

---

## Debate 3: The Intersectional Analysis Gap — Should Fairness Tools Return a Scalar or a Breakdown?

**Source:** [Trusted-AI/AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558)
**Project:** [IBM AIF360 — AI Fairness 360](https://github.com/Trusted-AI/AIF360)
**Tags:** `enhancement`, `intersectionality`
**Status:** Open since January 21, 2026 — unresolved

### The Question

The current implementation of the Empirical Differential Fairness (EDF) metric only returns a **single scalar value** summarizing fairness across protected attributes. But for intersectional analysis, a single number hides the story: which specific combinations of attribute groups contribute most to the maximum log-ratio?

jetverbeek asked for the metric to return the pair(s) of attribute groups that produce the max log ratio, plus a breakdown of all/top X group combinations.

**Side A — Single scalar is enough for most use cases.** Regulatory frameworks typically ask for a single fairness score per protected attribute.

**Side B — Intersectional analysis is essential for real fairness.** Hanabi9248 offered to implement a separate method for the group-pair breakdown while keeping the scalar return for backward compatibility.

**Why It Matters:** A single "fairness score" simplifies regulation but can erase the communities that need the most protection. Who decides the granularity of fairness reporting?

---

## Debate 4: The SHAP Measurement Problem — Does the Denominator Change the Story?

**Source:** [Fair-Code Issue #672](https://github.com/yakew7/Fair-Code/issues/672)
**Project:** [Fair-Code](https://github.com/yakew7/Fair-Code)
**Tags:** `documentation`, `good first issue`
**Status:** Open, unresolved

### The Question

When auditing racial bias in a COMPAS recidivism model using SHAP values, how should you report race's influence? The answer depends on a choice that isn't mentioned in the original write-up:

- **Option A — Top-5 features denominator:** Race accounts for **48.1%** of the top-5 features' combined influence
- **Option B — All-features denominator:** Race accounts for **42.3%** of all features' combined influence

Both numbers are correct. They answer different questions.

**Why It Matters:** The choice of denominator is a rhetorical decision disguised as a number. In a courtroom, a prosecutor would cite 48%. A defense attorney would cite 42%. A journalist would pick whichever fits the headline.

---

## Debate 5: The Impossibility Triangle — Which Fairness Metric Wins?

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

---

## How to Contribute a Debate

Found a great fairness controversy in an open-source issue thread? Contribute:

1. Link the issue/PR
2. Summarize the two (or more) sides
3. Explain *why* the debate matters beyond the repo
4. Add discussion prompts for our listeners

Format: follow the structure above — **The Question → Why It Matters → Discussion Prompts**

*Debates are sourced from real GitHub issue threads. The summary represents the maintainer's perspective; listener and contributor counterarguments are welcome — open an issue or submit a PR.*