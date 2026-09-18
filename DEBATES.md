# ⚖️ Live Debates from Open-Source Fairness Repos

Real controversies happening in real repos. Each entry is based on an actual open GitHub issue thread.

---

## 🔴 Debate #1: The Average-Odds Documentation Bug — Who Owns the Definition of "Zero"?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Opened:** April 23, 2024  |  **Last updated:** September 11, 2026  |  **Status:** Open  |  **👍 Reactions:** 2  |  **💬 Comments:** 1  
**Labels:** None (no maintainer has categorized or assigned it)

### The Claim

**AndreFCruz**, a developer and external contributor, filed an issue arguing that the `average_odds_difference` metric in AIF360 is **mathematically misdocumented**. The [API docstring](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> *"A value of 0 indicates equality of odds."*

Andre says this is **false**. He provides a visualization (attached to the issue) showing that there exist pairs of ROC curves where `average_odds_difference = 0` but **equalized odds do not hold**. The error, he points out, is not just in the AIF360 documentation — it also appears on **IBM's own fairness metrics webpage**.

### Why This Matters

The "average odds difference" metric is not an obscure corner case. It is one of the standard metrics shipped in the most widely used fairness toolkit in the world. If the documentation tells practitioners that zero means equality of odds — and that is not actually true — then:

1. **Audits can pass when they shouldn't.** A model could score 0 on average odds difference and still violate equalized odds, leading to a false sense of fairness.
2. **The error propagates.** IBM's website, tutorial notebooks, and downstream documentation all repeat the claim. Anyone learning fairness from AIF360 inherits the mistake.
3. **The definition of fairness is being silently distorted.** When a toolkit's docs are wrong about what a metric means, the toolkit shapes how an entire field thinks about fairness.

### The Response

After **17 months** with no maintainer response, **Hanabi9248** (a volunteer) commented on September 11, 2026:

> *"Could you assign this to me? The cancellation also appears in MetricTextExplainer and its JSON output. I have a focused correction for both API docstrings and the explanations, with a four-row example where average_odds_difference is 0 while average_abs_odds_difference and equalized_odds_difference are both 1. The metric formulas remain unchanged."*

This is a stunning admission in its own right: the error is **not just in the docs** — it's also in the `MetricTextExplainer`, the tool that generates human-readable explanations of fairness results. If the explainer says "this model achieves equalized odds" when it doesn't, the mismatch is not academic. It's operational.

### The Deeper Question: Who Decides What Zero Means?

This issue is really about **epistemic authority** in open-source fairness tools:

- **Is a metric what its formula says it is?** The formulas aren't changing — Hanabi9248 confirmed that. The math is fine. The problem is the *interpretation* attached to the result.
- **Does the documentation create reality?** If 1,000 practitioners read "A value of 0 indicates equality of odds" and trust it, does that make it true in practice — even if it's mathematically wrong?
- **Who bears the cost of a doc bug?** AndreFCruz filed it. Hanabi9248 offered to fix it. But no AIF360 maintainer has merged the correction in 17 months. The people closest to the fix are not the people with commit access.
- **Should fairness tools be more humble?** Maybe the real issue is that any single scalar claiming to capture "equality of odds" deserves a warning label — not a docstring that pretends it's definitive.

### Key Quotes from the Thread

> **AndreFCruz** (issue author): *"This mistake seems to be repeated on the IBM website"*  
> — Implying the error is systemic, not just a GitHub oversight.

> **Hanabi9248** (volunteer contributor): *"I have a focused correction for both API docstrings and the explanations, with a four-row example"*  
> — A concrete, implementable fix, offered and waiting.

### What's At Stake

| Stakeholder | What they lose |
|---|---|
| **Practitioner** | Trust in their fairness audit pipeline |
| **Affected community** | A "fair" model that isn't actually fair |
| **Open-source ecosystem** | Credibility of fairness tooling as a whole |
| **The concept of fairness** | Reduction to a number that doesn't mean what it says |

---

## 🔴 Debate #2: The Intersectional Analysis Gap — Should Fairness Tools Return a Scalar or a Story?

**Source:** [Trusted-AI/AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558)  
**Opened:** January 21, 2026  |  **Last updated:** September 16, 2026  |  **Status:** Open  |  **👍 Reactions:** 0  |  **💬 Comments:** 1  
**Labels:** None

### The Claim

**jetverbeek** opened an issue arguing that AIF360's `Empirical Differential Fairness` (EDF) metric is **incomplete for real-world fairness analysis**. The current implementation returns a **single scalar value** summarizing fairness across all protected attributes. But jetverbeek says this is inadequate for **intersectional analysis** — the practice of examining how multiple dimensions of identity (race × gender × disability, etc.) compound to create unique patterns of discrimination.

The request is specific:

> *"Extend the metric so that it returns: The pair(s) of attribute groups that produce the max log ratio. A breakdown of all/top X group combinations and their respective log-ratios."*

### Why This Matters

The single-scalar approach to fairness measurement has a well-documented blind spot: **intersectionality**. A model can have low overall disparity while still severely disadvantage a specific subgroup. For example:

- A hiring algorithm might show equal outcomes for "men" and "women" overall, but within the "women" group, **Black women** face a 40% higher false-negative rate than **white women**.
- A lending model might satisfy demographic parity globally, but **Black women with disabilities** are still denied at twice the rate of **white men without disabilities**.

The single scalar hides these layers. As jetverbeek puts it, for intersectional analysis it is "highly valuable to identify which combinations of attribute groups contribute most to the maximum log-ratio."

### The tension: Simplicity vs. Specificity

This debate exposes a fundamental design tension in fairness toolkits:

| Approach | Strength | Weakness |
|---|---|---|
| **Single scalar** | Easy to compare models, easy to report to leadership | Masks subgroup harm, invites " Pontius Fletcher" problems |
| **Group breakdown** | Reveals where harm concentrates, supports intersectional analysis | Hard to summarize, can overwhelm practitioners |

Neither approach is wrong. But the choice of which to ship first reflects a **value judgment** about who fairness tools are for:

- **For the CISO?** A single dashboard number.
- **For the civil rights attorney?** A breakdown of every group combination.
- **For the data scientist?** Both, with the ability to toggle.

### Why This Pairs with Debate #1

These two issues from the same repo tell a story:

1. **#528 says the tool's documentation is wrong** — the metric doesn't mean what the docs say it means.
2. **#558 says the tool's output is incomplete** — the metric doesn't show what the real world needs it to show.

Together, they suggest a pattern: **AIF360's fairness metrics are designed for a world where fairness is simple, when the real world is complex.** The toolkit gives you a number. But a number can be wrong (#528) and can also be insufficient (#558).

### The Unmet Need

jetverbeek's issue has been open for 8 months with no maintainer response and zero reactions. This could mean:

- **Low awareness:** Intersectionality isn't yet a priority in mainstream fairness tooling
- **High difficulty:** Extending EDF requires architectural changes to how metrics are returned
- **Bandwidth constraints:** AIF360 is maintained by IBM Research, not a dedicated open-source team
- **Philosophical disagreement:** Maybe the maintainers believe single-scalar metrics are sufficient

Whatever the reason, the silence itself is a data point. **When a community member asks for intersectional analysis and gets no response, the message is: this tool was not built with you in mind.**

### Key Quotes from the Thread

> **jetverbeek** (issue author): *"For intersectional analysis it would be highly valuable to identify which combinations of attribute groups contribute most to the maximum log-ratio."*  
> — A researcher or practitioner who has hit the limitation directly.

---

## 💡 How These Two Debates Connect

| Dimension | Debate #1 (#528) | Debate #2 (#558) |
|---|---|---|
| **Core problem** | The metric is mislabeled | The metric is incomplete |
| **Who found it** | External developer (AndreFCruz) | External contributor (jetverbeek) |
| **Who fixed it** | Volunteer (Hanabi9248) — waiting 17 months | No response yet — waiting 8 months |
| **What's broken** | Documentation & explainer | Output format (scalar vs. breakdown) |
| **Who's harmed** | Anyone who trusts "zero = fair" | Anyone whose intersectional identity is invisible in the data |
| **The meta-question** | Can you trust a tool's self-report? | Can a single number ever represent justice? |

---

## How to Add a Debate

Found a great controversy in an open-source fairness repo?:

1. **Open the issue** on GitHub and tag it `trusted-aI/aif360` or the relevant repo
2. **Summarize the core disagreement** in plain language (not just technical detail)
3. **Identify the stakes** — who wins and who loses depending on the outcome?
4. **Pull quotes from the thread** — let the participants speak for themselves
5. **End with a discussion prompt** — something listeners can argue about

Submit your summary by opening a PR to `DEBATES.md` or an issue tagged `debate-nomination`.

*Every debate in this file is a live, unresolved issue. These aren't history lessons — they're happening now.*