# 🔥 Live Debates from Open-Source Fairness Toolkits

Each entry summarizes a real, unresolved controversy pulled from an open issue thread in one of the projects listed in `RESOURCES.md`. These are the conversations that make great podcast episodes — where technical rigor meets social-justice values.

---

## Featured Debate: The Average-Odds Measurement Problem

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)
**Filed:** April 23, 2024 · **Last updated:** September 11, 2026 · **Reactions:** 👍 2
**Participants:** @AndreFCruz (reporter), @Hanabi9248 (correction contributor)

### The Claim

The AIF360 documentation for `average_odds_difference` states: *"A value of 0 indicates equality of odds."* But @AndreFCruz argues this is mathematically wrong. He demonstrates that any two points on a specific line in ROC space will yield `average_odds_difference = 0` **without** actually satisfying the equalized-odds criterion. The error, he says, is not just in the code comments — it's also reproduced on IBM's official fairness-metrics documentation page.

### Why This Matters

When a fairness toolkit's own documentation mislabels a metric, the consequences ripple outward:

- **Practitioners** who rely on the docs to choose which metric to optimize may select a criterion that doesn't actually guarantee the fairness property they think it does.
- **Auditors** citing AIF360 in an audit report may unintentionally overstate the fairness guarantees of a model.
- **Communities** subject to algorithmic decision-making deserve precision: "this model satisfies equalized odds" is a meaningful claim, and if the metric that's supposed to verify it is flawed, the claim is unreliable.

### The Thread So Far

1. **@AndreFCruz** posts the issue with a visual proof (an image showing a line in ROC space where average-odds = 0 but equalized odds is violated) and points out the same error appears in IBM's web documentation.
2. **@Hanabi9248** volunteers to fix it, noting the cancellation also appears in `MetricTextExplainer` and its JSON output. They have a concrete four-row example where `average_odds_difference = 0` while both `average_abs_odds_difference` and `equalized_odds_difference` equal 1. They clarify: the metric formulas themselves remain correct — it's the *explanation and docstrings* that are wrong.

### The Open Question

The issue is still open. Key tensions that the podcast could explore:

- **Precision vs. accessibility:** Fairness metrics are inherently technical. How do you document them precisely without alienating the practitioners who need them? Is it acceptable to offer simplified (but technically inaccurate) explanations for a broader audience?
- **Who verifies the verifiers?** AIF360 is maintained by IBM Research. When a community member finds an error in IBM's own fairness documentation, what's the right process for correction? How long should an issue stay open before it's treated as a priority?
- **The downstream harm:** If a mislabeled metric leads practitioners to deploy a model that doesn't actually satisfy equalized odds, who bears the harm? The toolkit author? The deploying organization? The affected community?

### Listen to This Episode's Discussion Prompt

> **If you discovered that a widely-used fairness metric in a popular toolkit was documented incorrectly — not because of a coding bug, but because the *explanation* of what the metric measures was wrong — how would you prioritize fixing it? Would you treat it as a documentation bug (low urgency), a correctness issue (medium urgency), or a potential harm vector (high urgency)? And who should be responsible for catching these kinds of errors: the toolkit maintainers, the users, or a third-party audit process?**

---

## Archive

Debates that have been covered in episodes and are now resolved or stale:

- *[To be added as episodes are released]*

---

## How to Nominate a Debate

1. Find an open issue in one of the projects in `RESOURCES.md` that surfaces a genuine disagreement (about metrics, definitions, trade-offs, or who a tool should serve).
2. Open an issue in this repo with the label `debate-nomination`.
3. Include: the issue link, a 2-paragraph summary of the disagreement, and why you think it matters for a social-justice audience.
