# AI Ethics Podcast — Ongoing Fairness Debates on GitHub

Real disputes from the fairness-tooling community that illuminate the tensions between ethical ideals and real-world deployment. Compiled for the **AI Ethics & Social Justice Podcast**.

---

## Debate: Does `average_odds_difference = 0` really mean "equalized odds"? — A fight over how to *define* a fairness metric

**Source:** [Trusted-AI/AIF360#528](https://github.com/Trusted-AI/AIF360/issues/528) (Opened Apr 2024, still open — unresolved)
**Reporter:** @AndreFCruz  |  **Product involved:** AI Fairness 360 (IBM)
**Core question:** *How do we measure fairness — and who gets to decide what a metric "means"?*

### The claim

The AIF360 documentation (and, reporter notes, an IBM-authoredwebinar) describes the `average_odds_difference` metric as "a value of 0 indicates equality of odds." @AndreFCruz argues this is **mathematically wrong**.

A value of `0` for average odds difference only guarantees that the ** averages** of the true-positive-rate and false-positive-rate gaps across groups are zero. It is perfectly possible for each group's TPR and FPR to differ **while those differences cancel out in the average** — producing `average_odds_difference = 0` even though **neither** equalized odds (nor even separate TPR/FPR parity) holds for any pair of groups.

Put concretely: two points on a line can average to zero without either being zero, so a "fair" average-odds score can mask real, group-specific misclassification disparities. The reporter flags the same error in an IBM-cited resource, suggesting the misstatement has spread beyond AIF360 into IBM's own corporate-facing fairness documentation.

### Why this matters beyond one library

1. **The "fairness by arithmetic" trap.** Auditors (and journalists) often pick a single metric, plug in a number, and call a model "fair" or "unfair." If the metric's definitional guarantee is itself wrong, every audit that treats a zero average-odds gap as proof of equalized odds is silently wrong — possibly legitimizing a discriminatory model.
2. **Metrics trade off against each other.** A well-known result (Chouldechova 2017; Kusner et al. 2017) shows that satisfying one fairness criterion can make another mathematically impossible for the same model. But that entire edifice rests on metrics meaning what they say. If a core metric is *mislabeled*, the claimed trade-off landscape shifts under your feet.
3. **Who defines the terms propagates into policy.** AIF360 is the reference implementation journalists and regulators cite. A documentation error in an IBM-backed, Apache-licensed toolkit doesn't stay in the repo — it shows up in compliance reports, audit dashboards, and courtroom testimony. The "who gets to define fairness" question is, here, answered by a docstring and a webinar slide.
4. **Open-source as a place where fairness is *argued*, not just computed.** This issue is a clean reminder that the fiercest fairness debates are not only in classrooms or courts — they're in public issue threads, where the engineers building the tools argue (sometimes for years) over first principles.

### Positions (as of the latest read)

- **@AndreFCruz (reporter / mathematician):** Stands by the definitional critique. The docstring claim and the IBM webinar are both incorrect; a zero average-odds difference is strictly weaker than equalized odds. The error is also present in an IBM-cited resource, amplifying its real-world reach. (Issue carries +2 reactions from community members who agree.)
- **Maintainers (Trusted-AI / IBM):** No response or correction posted in the thread — the issue remains open and unresolved. The silence is itself notable: a definitional error in a flagship metric has gone unaddressed in the reference implementation's public tracker.

### Open questions for listeners

- If a widely-cited library mislabels a metric, is the fix a docstring, or does it demand re-running every audit that used that metric as "equalized odds"?
- Is it acceptable for a reference fairness toolkit to carry a known-defective metric for years as long as nobody files a second issue?
- When the same toolkit is used by *both* journalists and defendants in algorithmic-harm cases, does a math-education failure become a justice problem?

This is a live, unpitched debate you can follow: [Trusted-AI/AIF360#528](https://github.com/Trusted-AI/AIF360/issues/528).

---

## How to Contribute

Know of another fairness debate unfolding on GitHub? Found a controversial issue thread where the community is wrestling with an ethical question? Open a PR or file an issue (use the `[DISCUSSION]` tag) with:
- The GitHub issue link
- A summary of the positions taken (fairly — represent all sides)
- Why it matters for the podcast

We prioritize debates that center the perspectives of communities most affected by algorithmic harm, not only technocrats and policymakers. When summarizing, quote directly when possible and link to exact comments rather than whole threads.
