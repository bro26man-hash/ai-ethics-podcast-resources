# ⚡ Ongoing Debates in the Fairness Community

Real GitHub issue threads that surfaced genuine, unresolved disagreements about how fairness tools should work — and who they should serve. Each entry is extracted from the source thread and contextualized for podcast discussion.

---

## 🔥 Debate #1: Should a Fairness Toolkit Try to Do Everything?

**Source:** [fairlearn/fairlearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — "MetricFrame should support metrics that don't require y_true and y_pred"
**Status:** 🟡 Open (74 comments, no resolution as of September 2025)
**Labels:** API

### The Core Question

Fairlearn's `MetricFrame` class currently only works with metrics that follow the scikit-learn signature `metric(y_true, y_pred)`. But there are many fairness-relevant metrics that don't fit this pattern:

- **Dataset-only metrics** (e.g., demographic parity, statistical parity difference) that just need `y_true`
- **Prediction-only metrics** (e.g., selection rate, false positive rate) that just need `y_pred`
- **Non-classification metrics** (e.g., contextual bandit rewards, cost-sensitive losses) that need completely different parameters like `actions`, `rewards`, `propensities`

The issue is whether to **remodel `MetricFrame` to support all of these** or **keep it simple and build separate tools for each problem type**.

### The Two Positions

#### 🔵 Position A: Expand MetricFrame (MiroDudik, Fairlearn maintainer)

MiroDudik argues that `MetricFrame` should be made more flexible:

1. **Make `y_true` and `y_pred` optional** (default to `None`) so metrics that don't need them can still be used
2. **Rename `metric` → `metrics`** for consistency with pandas and other arguments
3. **Add `sample_params`** as a way to pass extra arguments to metrics that need them

His reasoning:
- The current API is *"really limiting us"* and would require explaining "all kinds of workarounds" in the documentation
- Real use cases exist *outside* classification/regression: dataset-only metrics, streaming metrics, contextual bandits in lending, cost-sensitive learning
- A backward-compatible transition path exists (deprecation warnings first, then keyword-only args)

> *"I think that our current API is really limiting us, so I'd rather fix this sooner rather than commit to the future of docs that describe all kinds of workarounds."* — MiroDudik

#### 🟠 Position B: Keep MetricFrame Simple; Build Separate Tools (riedgar-ms, Fairlearn maintainer)

riedgar-ms argues that expanding `MetricFrame` is the wrong approach:

1. **`**kwargs` is dangerous** — it makes it impossible to ever add new named parameters without breaking someone
2. **Making `y_true`/`y_pred` optional opens "new failure modes"** that may not be user-friendly
3. **Different problem types need fundamentally different APIs** — subclassing or separate classes (e.g., `SupervisedMetricFrame`, `ReinforcementMetricFrame`) would be cleaner

His core principle:

> *"I think that MetricFrame does the fairly simple task of taking an sklearn-style (y_true, y_pred) metric, and turning it into one with grouping (via the sensitive factors). I'd prefer to keep it that way, and work out the best API for a new use case from a clean sheet."* — riedgar-ms

#### 🟢 Other Voices in the Thread

- **hildeweerts**: *"I am afraid that trying to fit all different kinds of learning tasks into one MetricFrame will become extremely confusing for novice users."* — Worried about the learning curve for new users.
- **romanlutz**: Asked for concrete examples before being convinced — *"I currently struggle to see the benefit of complicating the currently very intuitive API."*
- **MiroDudik** countered with 4 detailed scenarios showing exactly how the flexible API would work in practice (classification + AUC in one frame, multi-model comparison, streaming metrics, contextual bandits).

### Why This Matters for the Podcast

This debate is not really about Python API design. It's about **who fairness tools are built for** and **what "fairness" means in practice**:

1. **The novices vs. experts tension**: If you make the tool flexible enough for advanced researchers (contextual bandits, streaming metrics), do you alienate the practitioners and community advocates who need something simple and predictable?

2. **The "one size fits all" problem**: Is it possible (or desirable) to have a single fairness metric framework that works for hiring, lending, criminal justice, and recommendation systems? Or does each domain require its own approach — and its own community involvement?

3. **The backwards-compatibility trap**: Fairlearn's history shows that API changes can cause real harm (the gap between v0.4.6 and v0.5.0 left repo code incompatible with PyPI). This isn't just theoretical — it affects who can actually use the tool.

4. **The measurement question itself**: The debate assumes that the *same kind of disaggregation* (grouping by sensitive features and comparing rates/errors) applies across all these domains. But is measuring "demographic parity in hiring" the same kind of problem as measuring "inverse propensity scoring in contextual bandits"? The answer to that question determines whether one tool should serve both.

### Discussion Questions

- If you were maintaining Fairlearn, which side would you take — and why?
- Have you used a fairness tool that tried to do everything (and felt overwhelmed), or one that was too narrow (and couldn't handle your use case)?
- Should the *communities most affected by algorithmic harm* have a say in whether a tool is general-purpose or domain-specific?
- What does "accessibility" mean for a fairness tool — simple API, or domain-specific guidance?

---

## 📌 Debate #2: Coming Soon

We're always looking for the next great debate. Found a heated thread in AIF360, Aequitas, or another fairness project? [Open an issue](https://github.com/bro26man-hash/ai-ethics-podcast-resources/issues/new) and we'll curate it here.

---

*Debates are extracted from live GitHub issue threads. All positions are represented fairly and attributed to their original authors. The goal is not to declare a winner — it's to understand the sociotechnical tensions that make fairness work hard.*