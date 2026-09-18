# 🎙️ Episode Notes — "What Does Zero Mean?"

## Working Title: *The Metric That Lies by Telling the Truth*

### Logline
When the world's most-cited AI fairness toolkit ships a docstring that misstates what "zero" means, a volunteer tries to fix it — and waits 17 months. This is the story of AIF360 Issue #528, and what it reveals about who gets to define fairness.

### Sources
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The core debate
- [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Intersectional analysis gap
- [AIF360 Issue #548](https://github.com/Trusted-AI/AIF360/issues/548) — Website down since Feb 2025
- [Aequitas Issue #201](https://github.com/dssg/aequitas/issues/201) — Even the auditors need auditing
- [Aequitas Issue #209](https://github.com/dssg/aequitas/issues/209) — Versioning trust gap
- [Hardt et al. (2016)](https://papers.nips.cc/paper/6374-equality-of-opportunity-in-supervised-learning) — Original equalized odds paper
- [Chouldechova (2017)](https://arxiv.org/abs/1703.00056) — Impossibility theorem

### Key talking points
1. **The math is fine. The words are wrong.** AndreFCruz didn't argue the formula was broken — he argued the interpretation was. That distinction is everything.
2. **The explainer lies too.** `MetricTextExplainer` repeats the same error, meaning automated reports could certify unfair models as fair.
3. **17 months of silence.** A volunteer offered a complete fix. No maintainer merged it. What does that say about the governance of fairness tools?
4. **The intersectional irony.** While #528 sits unfixed, #558 asks for richer breakdowns. You can't add intersectional depth when single-axis definitions are already wrong.
5. **The pattern repeats.** Aequitas has the same documentation gap (#201) and a versioning trust problem (#209). This isn't one repo's problem — it's a field-wide crisis.

### Guest suggestions
- A maintainer from AIF360 or Aequitas (for the institutional perspective)
- AndreFCruz or Hanabi9248 (for the volunteer perspective)
- A legal scholar specializing in algorithmic liability
- A policymaker who uses these tools for regulatory decisions

### Counterarguments to surface
- "The formulas work fine — this is bikeshedding."
- "A volunteer fix can't substitute for maintainer governance."
- "The EU AI Act doesn't specify metric definitions — so does this matter at the regulatory level?"
- "If the documentation is wrong but the code is right, just read the code."

### Follow-up questions for listeners
See [the discussion issue](https://github.com/bro26man-hash/ai-ethics-podcast-resources/issues) in this repo for the official episode discussion prompt.
