# ⚖️ Live Debates from Open-Source Fairness Repos

Real controversies happening in real repos. Each entry is based on an actual open GitHub issue thread.

---

## 🔴 The Average-Odds Documentation Bug — Who Owns the Definition of "Zero"?

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

### Talking Points for the Episode

1. **The "zero" problem:** What does it actually mean for a fairness metric to equal zero? Is zero "perfectly fair" or just "not detectable"? And who gets to say?
2. **The explainer cascade:** When the metrics are right but the explanations are wrong, does the harm shift from the lab to the courtroom? A judge relying on MetricTextExplainer output might make a decision based on a false claim.
3. **Maintainer vacuum:** AIF360 is a flagship project. 2,866 stars. Apache-2.0. And a known documentation error has sat open for 17 months. What does it mean when the community identifies a fix but no one with merge access acts?
4. **The philosophical core:** Can any single metric ever fully capture "fairness"? Or is the real bug the assumption that fairness is a number?

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

### How to Listen

Play this episode alongside:  
🔗 [Read the full issue](https://github.com/Trusted-AI/AIF360/issues/528)  
🔗 [View the AIF360 metrics documentation](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html)  
🔗 [IBM's fairness metrics page](https://dataplatform.cloud.ibm.com/docs/content/wsj/model/wos-fairness-metrics-ovr.html)  
🔗 [Fair-Code's explainer: "Why Fairness Metrics Conflict"](https://github.com/yakew7/Fair-Code/blob/main/explainers/fairness-metric-conflicts.md)

### Discussion Prompt for Contributors

> **If you found a bug in how your fairness tool defines fairness — and a volunteer offered to fix it — how long should it take to ship? Who has the ethical obligation to act, and what are the consequences of inaction?**

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