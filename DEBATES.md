# 🔥 Open Debates in the Fairness Tooling Community

Real GitHub issue threads where the fairness community is actively disagreeing. Each entry includes background, the key positions, and why it matters for your work.

---

## Debate: Should Fairness Tools Recognize "Species" as a Sensitive Feature?

**Issue:** [fairlearn/fairlearn#1625](https://github.com/fairlearn/fairlearn/issues/1625)  
**Status:** Open (Feb 2026 – present, still unresolved)  
**Participants:** Final-year student (@samtuckerdavis) proposing the addition; two project maintainers (Tamara Atanasoska, Roman Lutz)

### Background

Fairlearn — Microsoft-backed, 2,284 stars, the most widely adopted Python fairness library — does not define a fixed list of sensitive features. Instead, users supply whatever grouping variable their context demands. A student argues that "species" should be treated as a sensitive feature alongside race, gender, and age, citing a growing peer-reviewed literature documenting measurable speciesist bias in AI:

- **Hagendorff et al. (2023)** found GPT-3 associated farmed animals with violence and explicitly called for fairness frameworks to include speciesist bias metrics.
- **Takeshita et al. (2022)** showed BERT and RoBERTa associate harmful words with nonhuman animals.
- **Hagendorff et al. (2025)** released SpeciesismBench (1,003 items): LLMs "frequently normalized harm toward farmed animals while refusing to do so for non-farmed animals."
- **Open Paws** built AHA Benchmark (4,350 items) measuring species-dependent risks of harm in LLM outputs.

The proposal asks for three concrete steps: documentation acknowledging speciesist bias, an example notebook demonstrating Fairlearn's existing MetricFrame with species as the sensitive feature, and eventually dedicated metrics.

### The Key Positions

| Stakeholder | Position | Reasoning |
|---|---|---|
| **Proposal author** (@samtuckerdavis) | Species should be recognized | Speciesism is measurable; the research base is growing; Fairlearn's mission is universal — "empower developers to assess and improve fairness" — and excluding one measurable form of unfairness contradicts that mission. | 
| **Maintainer @TamaraAtanasoska** (computational linguist) | Not in scope for Fairlearn at present | Fairlearn has no NLP components; sensitive features are user-supplied examples, not an exhaustive list; any column can be a sensitive attribute, so the "should we recognize it?" question is less urgent than it looks. |
| **Maintainer @RomanLutz** (also maintains Microsoft's PyRIT) | Redirect to PyRIT instead | PyRIT (the Python Risk Identification Toolkit) is better positioned for this application. Implies: fairness tools should specialize rather than be everything to everyone. |

### The Unresolved Tension

This issue crystallizes three questions that every fairness-tool builder must confront:

1. **Scope: Who should a fairness tool serve?** If the goal is "all sentient beings," is a purely demographic-parity toolkit sufficient, or does that toolkit implicitly endorse a species boundary? The proposal author argues that *not* flagging speciesist harm is itself a choice with moral weight. The maintainer argues that scope is a feature, not a bug — tools that try to do everything end up doing nothing well.

2. **Metrics: Can existing fairness metrics even detect species-based harm?** Fairlearn's demographic parity, equalized odds, and counterfactual fairness measures are designed for human demographic groups. Species classification in AI (trained on visual/linguistic features) may require fundamentally different metrics — the literature shows harm manifests as *normalized hostility* in LLM outputs, not merely as selection-rate disparities. New benchmarks (SpeciesismBench, AHA) suggest we need entirely new evaluation paradigms.

3. **Gatekeeping: Who decides what counts as a "fairness" issue?** The maintainer's redirect to PyRIT suggests a practical division of labor within the fairness ecosystem. But it also raises the question: what criteria determine which forms of bias get dedicated tool support, and who is excluded from that decision-making? Communities whose harm doesn't fit existing frameworks may never get representation at all.

### Why This Debate Won't Resolve Easily

- One side says *mission demands coverage*; the other says *scope enables focus*. Both are defensible.
- The research base is young and contested — the speciesism-in-AI papers are published in specialized venues, not yet mainstream enough to be consensus.
- The maintainers' identities matter: both Tamara Atanasoska and Roman Lutz are computational linguists. Their instinct to flag the NLP-vs-traditional-ML distinction reflects their expertise, but it may blind them to the moral argument.

### Discussion Prompts for Our Listeners

1. Should a fairness-auditing tool like Fairlearn covering only human demographics be considered incomplete — or is drawing a boundary around what to measure a practical necessity?
2. If you were designing a fairness toolkit today, would you include species as a sensitive feature out of the box, or would you make it user-supplied and defer to community contributions?
3. Does the maintainer's redirect to PyRIT reflect healthy ecosystem specialization, or does it risk creating an accessibility barrier where only projects with institutional backing can cover emerging bias categories?
4. Who should get to define what counts as a "form of unfairness" — the tool's maintainers, its users, the affected communities, the academic literature, or some combination?

---

## How to Submit a Debate

1. Find an open GitHub issue in an active fairness/bias-auditing tool that surfaces a genuine disagreement
2. Verify it's still active (comments within the last 30 days, not closed)
3. Summarize it in the format above: background, positions, unresolved tension, and 3–4 discussion prompts
4. Open a PR or file an issue in *this* repo with your submission

We prioritize debates that center communities most affected by algorithmic harm, not just technologist-to-technician disagreements.