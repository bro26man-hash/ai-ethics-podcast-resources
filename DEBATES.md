# ⚔️ Unresolved Debates in Fairness Tooling — Extracted from Real GitHub Issue Threads

These are genuine, ongoing disagreements from one of the most active open-source fairness communities. They are not theoretical — they shape the tools that real organizations use to measure and mitigate algorithmic bias.

---

## Debate #1: How Should We Measure Fairness? The `MetricFrame` API War

**Source:** [fairlearn/fairlearn issue #756](https://github.com/fairlearn/fairlearn/issues/756)
**Status:** Open, 74 comments, unresolved as of 2024
**Labels:** `API`

### The Core Tension

Fairlearn's `MetricFrame` is its flagship feature — it lets you compute fairness metrics disaggregated by sensitive attributes. But the API only supports metrics with the signature `metric(y_true, y_pred)` (i.e., standard classification/regression metrics).

The author and maintainer, **MiroDudik**, wants to expand `MetricFrame` to support:

1. **Metrics that don't use `y_true` and `y_pred`** — e.g., demographic parity (which only needs `y_pred`), streaming metrics, and metrics for contextual bandits or cost-sensitive learning.
2. **Flexible keyword arguments** — so users can pass arbitrary parameters without wrapping their metrics in adapter functions.

The proposed new API:
```python
# Current API
MetricFrame(metric, y_true, y_pred, *, sensitive_features, control_features=None, sample_params=None)

# Proposed API
MetricFrame(*, metrics, y_true=None, y_pred=None, sensitive_features, control_features=None, sample_params=None)
```

### The Three Sides of the Debate

#### 🟢 MiroDudik — "The tool should be as flexible as the problems we face"
- Argues that the current API is *"really limiting us"* and commits Fairlearn to a narrow definition of fairness measurement.
- Provides detailed examples (Scenarios 1–4) showing how real-world use cases — cost-sensitive learning, contextual bandits, streaming metrics — require metrics beyond `metric(y_true, y_pred)`.
- Proposes a gradual transition: deprecation warnings → keyword-only args → optional `y_true`/`y_pred`.
- Views this as essential for serving communities whose fairness problems don't fit the classification mold (e.g., resource allocation, lending with partial observations).

#### 🔴 riedgar-ms — "Don't break what works; build something new instead"
- Strongly opposes making `y_true` and `y_pred` optional: *"That is not how sklearn metrics work."*
- Argues `**kwargs` is a "really subtle" source of bugs that *"can elevate typos into really subtle bugs"*.
- Prefers to keep `MetricFrame` doing one thing well (disaggregated sklearn-style metrics) and create a **separate class** for other problem types.
- Accommodating — agreed to rename `metric` → `metrics` and add keyword args, but drew the line at optional `y_true`/`y_pred`.
- Frames this as a product question: *"What should MetricFrame be?"* — not just an API question.

#### 🟡 hildeweerts & romanlutz — "Show us examples before committing; protect novices"
- Both maintainers who initially opposed the change, then warmed to Alternative A (optional `y_true`/`y_pred`) *only after* MiroDudik provided concrete scenarios.
- Expresses concern that Alternative B (`**kwargs`) introduces *"too much magic"* that makes code harder to read and debug.
- Emphasizes that the *"majority of users will be looking for classification/regression metrics"* and that complicating the API for edge cases may overwhelm newcomers.
- Partners with riedgar-ms in wanting a clean migration path (deprecation warnings before breaking changes).

### The Deeper Philosophical Fault Lines

| Question | MiroDudik's position | riedgar-ms's position |
|----------|----------------------|-----------------------|
| What is `MetricFrame` for? | A general-purpose fairness measurement tool for any ML paradigm | A specialist tool for disaggregated sklearn-style metrics |
| Who should it serve? | Advanced users with non-standard fairness problems | Novice users who need an intuitive, predictable API |
| How do you handle trade-offs? | Flexibility through optional arguments and `sample_params` | Separation of concerns — new class for new problem types |
| What's the cost of complexity? | Worth it to avoid forcing users into awkward workarounds | Hidden bugs, documentation burden, and broken existing code |

### Why This Matters for Our Podcast

This debate is not just about Python API design. It's about:

- **What counts as a fairness problem?** When the tool only handles `y_true`/`y_pred` metrics, it implicitly defines algorithmic harm as something that occurs in classification/regression. But the most consequential algorithmic decisions — resource allocation, predictive policing won't be caught, welfare eligibility — don't fit that mold.
- **Who gets to define fairness metrics?** The maintainers of a widely-used tool are making political decisions about which fairness definitions and measurement approaches are accessible. Their API choices shape what the broader community can even *ask* of an algorithm.
- **Accessibility vs. ambition.** A tool that's easy for beginners to use may not serve the communities with the most complex fairness needs. A tool that's powerful and flexible may be impenetrable to non-technical stakeholders.

---

*More debates to be added. Found a controversial fairness issue thread? Open an issue in this repo!*