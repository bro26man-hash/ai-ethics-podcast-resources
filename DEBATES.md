# AI Ethics Podcast — Ongoing Fairness Debates on GitHub

Real disputes from the fairness-tooling community that illuminate the tensions between ethical ideals and real-world deployment. Curated for the **AI Ethics & Social Justice Podcast**.

---

## Debate: Should a Fairness Tool Recognize "Species" as a Sensitive Feature for Bias Evaluation?

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
While sympathetic to the cause, the maintainer raised three substantive objections:
1. **Domain mismatch.** All the cited research concerns NLP and language models, while Fairlearn has no NLP components and is designed for traditional (tabular, classification) ML. The tool's architecture and metric suite are not built for text-based bias signals.
2. **No fixed sensitive-feature list.** Fairlearn deliberately does not define a canonical list of sensitive attributes in code — any column can become a sensitive feature. The documented list in the User Guide is *non-exhaustive* — just a few illustrative examples ("race, gender, age, etc."). Adding species would imply formalizing it in a way that contradicts this design philosophy.
3. **Opportunity cost.** Documentation and example notebooks are finite resources. Adding a highly specialized topic may distract from the core fairness dimensions (race, gender, socioeconomic status) that the vast majority of practitioners need.

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

## How to Contribute

Know of another fairness debate unfolding on GitHub? Found a controversial issue thread where the community is wrestling with an ethical question? Open a PR or issue with:
- The GitHub issue link
- A summary of the positions taken
- Why it matters for the podcast

We prioritize debates that center the perspectives of communities most affected by algorithmic harm, not just technologists and policymakers.
