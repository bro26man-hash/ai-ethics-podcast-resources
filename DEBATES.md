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

4. **The measurement question itself**: The debate assumes that the *same kind of disaggregation* (grouping by sensitive features and comparing rates/errors) applies across all these domains. But is measuring "demographic parity in hiring" the same kind of problem as measuring "inverse propensity sampling in contextual bandits"? The answer to that question determines whether one tool should serve both.

---

## 🔥 Debate #2: Does a Popular Fairness Metric's Documentation Mislead Practitioners About What It Actually Measures?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — "`average_odds_difference` metric is wrongly represented as an equalized odds relaxation"
**Status:** 🟡 Open (as of April 2024, still unresolved — 1 maintainer comment, Sep 2026)
**Opened by:** [@AndreFCruz](https://github.com/AndreFCruz) | **Reactions:** 👍 2

### The Core Question

In AIF360's official documentation, the `average_odds_difference` metric is described as: *"A value of 0 indicates equality of odds."* This is a significant claim — it tells practitioners that if they see a score of 0, their model satisfies the equalized odds criterion, a widely-used fairness definition requiring that true positive rates and false positive rates be equal across demographic groups.

But AndreFCruz, a careful reader and practitioner, argues that **this is mathematically wrong**. They demonstrate (with a geometric visualization) that there exist configurations where `average_odds_difference = 0` yet the model *clearly does not* satisfy equalized odds. The documentation, they contend, is not merely imprecise — it is **misleading in a way that could cause real harm**. A practitioner reading the docs might conclude their model is fair when it is not, leading to false confidence and potentially discriminatory decisions deployed at scale.

The issue also notes that this same incorrect characterization appears on **IBM's published fairness metrics documentation**, extending the problem from the open-source library to its commercial and educational spin-offs.

### The Three Positions

#### 🔵 Position A: Documentation Accuracy Is a Dependency, Not a Detail

When a fairness toolkit tells users that a metric value of 0 means "you are fair," that is a *claim about the world*. If that claim is wrong, the entire trust architecture of the toolkit collapses. Practitioners build pipelines, run audits, sign off on compliance — all based on what the documentation says the numbers mean. An incorrect mathematical claim in docs isn't a typo; it's a structural failure that can propagate through real decision-making systems affecting real people's lives.

#### 🟠 Position B: The Metric Naming Itself Is the Deeper Problem

Others argue that the issue isn't just documentation but the *choice of metric itself*. The name `average_odds_difference` suggests it measures odds differences, but equalized odds is a specific, well-defined mathematical property. If the metric doesn't actually measure that property, the naming itself is the problem — not just the docstring. This camp would argue the fix should be renaming the metric or creating a new one, not just tweaking the documentation.

#### 🟢 Position C (Maintainer's View): Fix the Docs, But This Reveals a Deeper Design Question

Hanabi9248, an AIF360 maintainer, responded to the issue offering to assign themselves the fix, noting that the same error appears in the `MetricTextExplainer` and its JSON output. They have a "focused correction" with a four-row example where `average_odds_difference=0` while both `average_abs_odds_difference` and `equalized_odds_difference` are 1. The metric formulas remain unchanged — the fix is documentation-only. But this raises the question: *how did this error survive peer review, Bayesian testing, and years of citations?* And more importantly: what other claims in the toolkit's 70+ metric documentation might need similar scrutiny?

### Why This Matters for the Podcast

This issue exposes a fault line that runs through the entire fairness tooling ecosystem:

1. **The gap between mathematical rigor and engineering pragmatism**: Academic fairness metrics are defined with mathematical precision, but toolkit implementers must make choices about naming, documentation, and default behavior that affect how non-mathematicians understand them. Every abstraction layer introduces the possibility of distortion.

2. **Accountability of toolkit maintainers**: When IBM Research's toolkit tells a bank's compliance officer that their model is "fair" based on a metric score, who bears responsibility when that metric is later shown to be misdocumented? The maintainer? The institution? The user?

3. **The replication crisis in fairness research**: If a widely-used metric's documentation is wrong, how many research papers have cited it as evidence that a model "satisfies equalized odds" when it doesn't? The implications for the academic literature are profound.

4. **Who gets harmed by imprecise documentation**: The answer is not abstract. It's the communities whose lives are affected by algorithmic decisions — the people who never see the metric score, never read the docstring, but who bear the consequences when "fair" means something different than what they were told.

### Discussion Questions

- If you were a compliance officer at a bank using AIF360, how would you verify that the metric documentation is correct — and what would you do if you couldn't?
- Should toolkit maintainers be held to a standard of mathematical proof for every claim in their documentation? Who would enforce that?
- Is it possible for a single metric to truly "indicate" a complex fairness property like equalized odds? Or does reducing a multidimensional concept to one number inevitably produce a misleading story?
- If you discovered that a fairness metric you'd been citing in your research was misdocumented, how would you respond? Would you retract, correct, or wait for the maintainers to fix it?

---

## 📌 Found a Debate?

Found a heated thread in AIF360, Aequitas, Fairlearn, or another fairness project? [Open an issue](https://github.com/bro26man-hash/ai-ethics-podcast-resources/issues/new) in this repo and tag it with `[DEBATE]` — we'll curate it here.

*Debates are extracted from live GitHub issue threads. All positions are represented fairly and attributed to their original authors. The goal is not to declare a winner — it's to understand the sociotechnical tensions that make fairness work hard.*
