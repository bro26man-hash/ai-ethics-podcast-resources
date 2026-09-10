# AI Ethics Podcast — Ongoing Fairness Debates on GitHub

Real disputes from the fairness-tooling community that illuminate the tensions between ethical ideals and real-world deployment. Curated for the **AI Ethics & Social Justice Podcast**.

---

## Debate 1: Should a Fairness Tool Recognize "Species" as a Sensitive Feature for Bias Evaluation?

**Source:** [fairlearn/fairlearn#1625](https://github.com/fairlearn/fairlearn/issues/1625) (Opened Feb 2026, still open as of Jul 2026)

**Context:** A community contributor proposed adding "species" (animal species) as a recognized sensitive feature in Fairlearn's fairness evaluation framework, citing a growing body of peer-reviewed research documenting speciesist bias in AI systems. What followed was a nuanced disagreement between a community member and two maintainers about what a fairness library *should* cover, who its audiences are, and where to draw the boundary between "bias we resolve to measure" and "bias we treat as someone else's problem."

### Core Positions

**Pro-Inclusion — @samtuckerdavis (community contributor):**  
The proposal is grounded in specific, peer-reviewed research:
- **Hagendorff, Bossert, Tse & Singer (2023).** *"Speciesist bias in AI."* AI and Ethics. Found GPT-3 associates farmed animals with violence; explicitly calls for fairness frameworks to include speciesist bias metrics. [DOI: 10.1007/s43681-023-00380-w](https://doi.org/10.1007/s43681-023-00380-w)
- **Takeshita et al. (2022).** BERT and RoBERTa associate harmful words with nonhuman animals. *Information Processing & Management.*
- **Hagendorff et al. (2025).** SpeciesismBench (1,003 items): LLMs "frequently normalized harm toward farmed animals while refusing to do so for non-farmed animals."
- **AI-for-Animals (2025).** AHA Benchmark (4,350 items): species-dependent risks of harm in LLM outputs.

The contributor argues that Fairlearn — a Microsoft-backed project with strong academic credibility — acknowledging species-based discrimination would set an important precedent. They also pledged to contribute documentation, example notebooks, or metric implementations themselves.

**Respectful Skepticism — @TamaraAtanasoska (maintainer, computational linguist):**  
While sympathetic, the maintainer raised three substantive objections:
1. **Domain mismatch.** All the cited research concerns NLP and language models, while Fairlearn has no NLP components and is designed for traditional (tabular, classification) ML. The tool's architecture and metric suite are not built for text-based bias signals.
2. **No fixed sensitive-feature list.** Fairlearn deliberately does not define a canonical list of sensitive attributes in code — any column can become a sensitive feature. The documented User Guide list is *non-exhaustive* — just illustrative examples ("race, gender, age, etc."). Adding species would imply formalizing it in a way that contradicts this design philosophy.
3. **Opportunity cost.** Documentation and example notebooks are finite resources; adding a highly specialized topic may distract from the core fairness dimensions (race, gender, socioeconomic status) that the vast majority of practitioners need.

**Cross-Pollination Suggestion — @romanlutz (maintainer, PyRIT team):**  
"This is very much in scope for [Microsoft's PyRIT](https://github.com/microsoft/PyRIT) library!" — pointing the contributor toward a broader responsible-AI toolkit that covers NLP, generative AI, and multiple bias dimensions including species, rather than directing the effort at Fairlearn specifically.

### The Unresolved Tension

This debate crystallizes three questions that remain deeply relevant to the entire fairness-tooling ecosystem:

1. **Where does one fairness tool's responsibility end and another's begin?** Fairlearn covers traditional ML; PyRIT covers NLP and generative AI. But the boundary isn't always clean — the same biased model might be audited through different lenses depending on who's using which tool. Does splitting responsibilities across projects make fairness *more* manageable, or does it create gaps where no one feels accountable?

2. **Should fairness tools be "domain-general" or "domain-specific"?** A tool that tries to cover everything risks shallow treatment of each domain. A tool that covers one domain well risks excluding affected communities whose bias doesn't fit that domain. Fairlearn's choice to be domain-general (any column can be sensitive) while also curating illustrative examples creates this exact tension.

3. **Who gets to define what counts as "fairness"?** The contributor brought citations from animal-welfare researchers arguing speciesist bias is measurable harm. The maintainers responded that *their* community (traditional ML practitioners) has different priorities. This question — who defines the fairness framework, and whose expertise is centered — isn't just about species. It's about race, gender, disability, and every axis along which AI systems can cause harm.

4. **Is "not now" the same as "never"?** When maintainers say this isn't in scope for Fairlearn but is for PyRIT, they're making a practical decision about project resources. But for the communities affected by speciesist AI harm, "not in scope" can feel like invisibility. How should open-source fairness projects communicate trade-offs without reinforcing the very power asymmetries they aim to address?

### Why This Matters for Listeners

This isn't an abstract technical disagreement. The answer to "which forms of bias should our tools cover?" determines whether marginalized communities are visible in the fairness infrastructure — or rendered invisible by a tool that only looks the way *its creators* think to look. The Fairlearn community's navigation of this question — listening respectfully, explaining their technical constraints, and redirecting to a more relevant project — is a model for how open-source ethics code *could* work. But it's also a reminder that redirection only works if the redirected project actually acts on it.

This is a live debate you can follow: [fairlearn/fairlearn#1625](https://github.com/fairlearn/fairlearn/issues/1625).

---

## Debate 2 (Supplementary): Does a "Zero" Fairness Metric Actually Mean the Model Is Fair? — AIF360 #528

**Source:** [Trusted-AI/AIF360#528](https://github.com/Trusted-AI/AIF360/issues/528) (Opened Apr 2024, still open, 2 👍 endorsements)

**Context:** A contributor opened a sharp technical objection against how AIF360 characterizes its own `average_odds_difference` metric. The docs state it is "A value of 0 indicates equality of odds." The contributor — backed by a simple geometric argument — says that's not true: there exist distinct (FPR, TPR) pairs for which `average_odds_difference = 0` but equalized odds does *not* hold. That same error, they note, is reproduced on IBM's own website documentation. The issue has no maintainer response yet — a silence worth listening to.

**The critic's claim:** Equalized odds requires both equal true-positive rates *and* equal false-positive rates across groups. `average_odds_difference` averages the two odds differences, so it's possible for one to be positive and the other negative, canceling to zero while both remain unequal. Zero here masks continuing harm. Presenting it as an "equality of odds" metric is, at best, misleading — and it appears in official IBM documentation, meaning the misconception is being circulated to enterprise practitioners.

**What's at stake:** This is the "how to measure" leg of the episode. A metric that reads "0 = fair" can deliver false reassurance of fairness to a group that is actually being harmed in opposite directions across its two error rates. For an auditing tool, getting the *semantics* of a "zero" right isn't pedantry — it's the difference between a dashboard that exposes injustice and one that hides it behind a reassuring number.

**Sub-questions for listeners:**
- If the most-widely-used fairness toolkit mislabels what its flagship metric means, how much trust can we place in "out-of-the-box" fairness reports?
- Who's checking the documentation against the math — the auditing team, or the publisher's marketing/demo site?
- Should a "0 = fair" claim in a tool ever be made without an explicit, exposed proof/counterexample so users can't be silently misled?

**Why it pairs with Debate 1:** Debate 1 asks *who* a fairness tool should serve and *which harms* it should cover. Debate 2 asks, once you've picked a harm and a group, *whether your ruler even measures correctly*. Together they bracket the fairness-auditing enterprise: scope (the "who/which"), and measurement validity (the "how well"). A tool can fail at both, and when it fails at measurement, it often fails *for* the very communities it claimed to help.

This debate is live and awaiting a maintainer response: [Trusted-AI/AIF360#528](https://github.com/Trusted-AI/AIF360/issues/528).

---

## How to Contribute

Know of another fairness debate unfolding on GitHub? Found a controversial issue thread where the community is wrestling with an ethical question? Open a PR or issue with:
- The GitHub issue link
- A summary of the positions taken
- Why it matters for the podcast

We prioritize debates that center the perspectives of communities most affected by algorithmic harm, not just technologists and policymakers.
