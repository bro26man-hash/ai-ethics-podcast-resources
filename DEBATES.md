# ⚡ Ongoing Debates in AI Fairness

A living document summarizing real controversies from open-source fairness toolkits — perfect material for podcast deep-dives.

---

## 🔥 Debate #1: How Should Fairness Metrics Handle Non-Standard Data?

**Source:** [fairlearn/fairlearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756)
**Labels:** `API` | **Status:** Open (74 comments, no resolution)
**Date opened:** April 2021 | **Last updated:** September 2021 (still open!)

### The Core Question

Microsoft's Fairlearn toolkit measures fairness by comparing model predictions across demographic groups. Its central tool, `MetricFrame`, was designed for standard classification/regression metrics that take `(y_true, y_pred)` as arguments. But real-world fairness evaluation often needs metrics that *don't* fit that pattern — for example:

- **Contextual bandit fairness**: In lending, you only observe repayment (reward) for the loan type you offered (action), not for counterfactual alternatives.
- **Cost-sensitive learning**: The metric depends on cost matrices, not predicted labels.
- **Streaming metrics**: Data arrives continuously; you don't have a fixed dataset to score against.
- **Dataset-only metrics**: Metrics like demographic parity only need the dataset, not a model's predictions.

The question: **Should Fairlearn's `MetricFrame` be redesigned to support these non-standard metrics, or should separate tools be built for different problem domains?**

### The Two Positions

#### 🟢 Position A: Expand MetricFrame (MiroDudik, Fairlearn maintainer)

- Make `y_true` and `y_pred` *optional* keyword arguments (defaulting to `None`).
- This lets users pass any metric signature, from classification to bandits, without "dummy" arguments.
- **Key quote:** *"The current API is really limiting us, so I'd rather fix this sooner rather than commit to the future of docs that describe all kinds of workarounds."*
- **Underlying philosophy:** A fairness tool should be a *universal framework* for evaluating any ML system, regardless of problem type. Flexibility matters more than simplicity.

#### 🔴 Position B: Keep MetricFrame Focused (riedgar-ms, hildeweerts, other maintainers)

- `y_true` and `y_pred` should remain **required** arguments. Making them optional opens new failure modes and dilutes the tool's clarity.
- `**kwargs` (the proposed way to pass arbitrary parameters) is dangerous: any future parameter name could collide with a user's keyword, creating subtle bugs.
- Different problem types (reinforcement learning, NLP, vision) have fundamentally different metric needs — they deserve *separate classes*, not a bloated generalist.
- **Key quote (hildeweerts):** *"The majority of users will be looking for classification/regression metrics and may not even be familiar with reinforcement learning. Having to look at examples to understand how to use an API even in the 'simplest' scenario signals that it is not intuitive."*
- **Underlying philosophy:** A fairness tool should do *one thing well* and stay accessible to non-experts. "Capable of everything" often means "confusing for everyone."

### Where It Ended Up

The community eventually converged on a **compromise ("Alternative A")**:
- `y_true` and `y_pred` become *optional* (keyword-only, defaulting to `None`).
- No `**kwargs` — a `sample_params` dictionary still handles extra arguments.
- This covers streaming, bandit, and cost-sensitive scenarios without full `**kwargs` flexibility.

**But the issue remains open.** The philosophical tension — between generality and clarity, between power users and novices — has not been resolved. As of the latest update, this debate has been running for **over three years**.

### 🎙️ Podcast Talking Points

1. **Who does a fairness tool serve?** The maintainer argues for liberty-loving data scientists who need flexibility; his colleagues argue for the typical user who just wants a simple, predictable API.
2. **Can "fairness" be measured a single way?** The existence of 10+ competing fairness metrics (demographic parity, equalized odds, predictive parity…) suggests there's no neutral answer — each metric encodes a different theory of justice.
3. **Does tool design reflect power dynamics?** When a maintainer pushes for `**kwargs` and others resist, is that a technical debate — or a power struggle over who gets to define what "fairness" means?
4. **The long tail of open issues:** This issue has been open since 2021. What does it mean that the fairness community spends years debating API design while real-world biased algorithms keep running?

---

## 🔥 Debate #2: What Do Fairness Metrics Actually Measure? The Utility-vs-Fairness War Inside AIF360

**Source:** [Trusted-AI/AIF360 Issue #214](https://github.com/Trusted-AI/AIF360/pull/214) (and extended discussion in comments)
**Labels:** `metrics` | **Status:** Open (9 comments, over 3 years)
**Date opened:** November 2020 | **Last updated:** February 2023

### The Core Question

AIF360 categorizes several "inequality indices" (like the Generalized Entropy Index, GEI) as *individual fairness metrics*. But a contributor named **leenamurgai** made a startling claim: **these indices don't measure fairness at all — they measure utility.** This isn't a refactoring suggestion; it's a fundamental challenge to how the toolkit classifies its own measurements.

### The Three Positions

#### 🟢 leenamurgai — "These are utility metrics disguised as fairness metrics"

- Submitted a long mathematical proof showing that GEI can be rewritten as a function of two parameters: **model accuracy (λ)** and **mean benefit (μ)** — both utility concepts, not fairness concepts.
- Proved that minimizing GEI is equivalent to minimizing cross-entropy loss (when α=0) or mean squared error (when α=2) — both standard ML optimization objectives, not fairness objectives.
- Argued that the two "individual fairness" definitions in the literature are **contradictory at a conceptual level**: Dwork et al. (2011) defines individual fairness as a property of a *single map* (ground truth provided by a similarity metric), while Speicher et al. (2018) defines it via distributional comparisons. You can't classify both as "individual fairness."
- **Key quote:** *"It is misleading to have GEE categorised under individual fairness metrics alongside consistency. Individual fairness as defined by Dwork et al. is importantly not a measure of utility."*
- Concluded that the famous "fairness-utility trade-off" paper isn't showing a trade-off between group fairness and individual fairness at all — it's just showing the well-known utility-vs-fairness trade-off.
- **Political implication:** If we miscategorize utility as fairness, we risk *selling* utility optimization to policymakers as if it were fairness enforcement. "Choosing a benefit function is choosing whose interests count as 'beneficial' — and that's a political decision, not a technical one."

#### 🟡 hoffmansc — "They measure a different kind of fairness — let's call it 'distributional fairness'"

- Agreed that GEI doesn't match Dwork et al.'s definition of individual fairness.
- But pushed back on the claim that it's *only* utility: GEI has no dependence on the feature space X (it's "anonymous" — only looks at predictions and ground truth), which is similar to utility functions but could still measure a *distributional* property.
- Proposed reclassifying these indices as **"distributional fairness"** metrics — a separate category from both individual fairness and group fairness.
- Asked practical questions: Should we calculate overall GEI or only between-group? What about within-group? These questions have real implications for how auditors interpret results.
- **Key quote:** *"Perhaps we could categorize them separately as, say, 'distributional fairness'?"*
- Showed willingness to accept leenamurgai's findings and update documentation, but wanted careful treatment.

#### 🔴 The Implicit Position — "This doesn't matter in practice"

- The debate has received **9 comments over 3 years** and remains unresolved. Several core maintainers of AIF360 (including Kurt Varshney, Michael Hind) have not weighed in on the mathematical substance.
- The metrics remain categorized as "individual fairness" in the documentation and sklearn API.
- **What this silence means:** Either the maintainers haven't engaged with the critique, or they consider it too theoretical for practical impact. Both possibilities are concerning for a toolkit used by real auditors and policymakers.

### Why This Debate Matters for Social Justice

This isn't an abstract math dispute. It has real consequences:

1. **Misleading governance:** If AIF360 labels a utility metric as "individual fairness," an auditor using the toolkit might believe they are enforcing a rigorous fairness standard when they are merely optimizing model performance. This could lead to false certification of biased systems as "fair."

2. **Whose interests count?** The benefit function at the heart of GEI encodes *whose outcomes count as beneficial*. When leenamurgai asks "is it meaningful to ignore people if you're trying to be fair?", he's asking whether fairness metrics should consider *everyone* affected — or only the groups explicitly designated as sensitive attributes. If a welfare algorithm's benefit function only looks at qualification rates, it may miss the community that considers *being considered human* the baseline benefit.

3. **The language problem:** The debate about whether to call these metrics "fairness" or "utility" echoes the broader argument about whether a tool can even "remove bias." If we can't agree on what our tools measure, how can we trust what they tell us?

### 🎙️ Podcast Talking Points

1. **The marketing problem:** Fairness toolkits sell "fairness" to companies and governments. If the metrics inside are really measuring utility, the product is fundamentally misrepresented. Who bears the cost of that misrepresentation?
2. **The "Who is the customer?" question:** Fairlearn maintainers debate whether MetricFrame should serve data scientists or the communities affected by models. AIF360 maintainers haven't weighed in on whether their metrics serve auditors or utility optimizers. Are these the same question?
3. **The silence is also a position:** When senior maintainers don't respond to a 3-year-old mathematical critique, what message does that send about the community's commitment to intellectual honesty?
4. **The Dwork vs. Speicher paradox:** Two foundational papers define "individual fairness" in contradictory ways. Both are widely cited. Neither textbook reconciles them. How did the field get here?

---

## 🔥 Debate #3: Should Fairness Tools Measure Harm to Non-Human Animals? The Speciesism Proposal Inside Fairlearn

**Source:** [fairlearn/fairlearn Issue #1625](https://github.com/fairlearn/fairlearn/issues/1625)
**Labels:** `proposal` | **Status:** Open (2 comments across 4 months, unresolved)
**Date opened:** February 25, 2026 | **Last updated:** July 1, 2026

### The Core Question

A contributor named **samtuckerdavis** proposed that Fairlearn — Microsoft's fairness toolkit — should recognize **species** as a valid sensitive feature for fairness evaluation. The argument is backed by peer-reviewed research showing that AI systems exhibit measurable "speciesist bias" against farmed animals:

- Hagendorff, Bossert, Tse & Singer (2023): GPT-3 associates farmed animals with violence
- Takeshita et al. (2022): BERT, RoBERTa associate harmful words with nonhuman animals
- Hagendorff et al. (2025): SpeciesismBench (1,003 items) — LLMs "frequently normalized harm toward farmed animals"
- AI-for-Animals (2025): AHA Benchmark (4,350 items) — species-dependent risks of harm in LLM outputs

The proposal suggests: (1) documenting speciesist bias as a recognized category, (2) tutorial notebooks demonstrating MetricFrame with species as a sensitive feature, and (3) future dedicated metrics for speciesist bias in NLP models. The proposer also offers to contribute from Open Paws, an organization that builds AI tools for animal advocacy.

**The deeper question:** Who — or *what* — should AI fairness tools serve?

### The Two Positions

#### 🟢 Speciesism as Fairness (samtuckerdavis — external researcher, Open Paws affiliated)

- Fairlearn's mission is "to empower developers to assess and improve the fairness of AI systems." If species-based discrimination is a measurable form of unfairness in AI (as peer-reviewed research demonstrates), then fairness tools should acknowledge it.
- A Microsoft-backed project acknowledging speciesist bias would "set an important precedent" — signaling that the scope of fairness is not coextensive with human demographic categories.
- The existing Fairlearn infrastructure (MetricFrame, sensitive attribute columns) can already support this without code changes — it just needs documentation and examples.
- **Key framing:** Fairness is not intrinsically limited to *humans*. If it were, the tool would be encoding a species boundary that itself privileges one category of sentient being over another — the same kind of boundary that fairness metrics were designed to examine.

#### 🔴 Fairlearn Is Not for This (TamaraAtanasoska & romanlutz — Fairlearn maintainers)

- **TamaraAtanasoska** (maintainer, computational linguist): Acknowledged the value of the research but noted that Fairlearn is currently focused on *traditional machine learning* and has no NLP components. Fairlearn has not one, but *no* hardcoded list of sensitive features — "any column of information can become a sensitive attribute or a proxy for it." In her view, the question isn't whether Fairlearn *can* measure speciesist bias, but whether it should add a *dedicated category* for it when the existing framework already handles it by simply treating any column as a sensitive feature.
- **romanlutz** (maintainer of microsoft/PyRIT): Agreed with Tamara and suggested that speciesist bias evaluation falls more naturally under PyRIT (Responsible AI Risk Toolkit) — Microsoft's newer threat-modeling tool for generative AI — rather than Fairlearn, which is designed for classical ML fairness metrics.
- **Underlying philosophy:** Specialized tools should do their core job well. Adding speciesist bias as a named category in Fairlearn risks bloating the toolkit with domain-specific concerns that existing tools (PyRIT for NLP/LLM risks, or standard Fairlearn usage with a Species column) already handle without a named category.

### Why This Debate Matters for Social Justice

This is not a quirky edge case. It cuts to the **core philosophical tension** in fairness tooling:

1. **The boundary of moral considerability.** Every fairness toolkit decides which groups count as "sensitive" and which harms count as "unfair." When Fairlearn was designed, its creators chose to focus on human demographic groups. The speciesism proposal asks: *Was that choice principled, or just default?* If the tool's boundary was set by its human creators — at Microsoft, a company with massive animal agriculture supply chain exposure and a foundation program funding animal welfare — then the boundary is itself a sociopolitical decision that has never been transparently justified.

2. **Tool design limits the conversation.** A fairness tool that only measures bias against *humans* implicitly tells its users: "The only beings worth auditing for are humans." This is not a neutral technical choice — it is an act of *scoping* that shapes what its users think of as a fairness problem. The speciesism proposal is a direct challenge to that scoping decision.

3. **Who decides when "fairness" expands?** Fairlearn maintainers routed the speciesism proposal toward a different tool (PyRIT) rather than expanding Fairlearn's scope. This raises a governance question: **who has the authority to decide what a fairness tool is for?** The maintainers? The corporate funder (Microsoft)? The academic contributors? The communities affected by bias? If the answer is "the maintainers and their corporate funder," then the tool's scope is less a technical decision than a consequence of institutional power.

4. **The "species boundary" as bias.** Dwork et al.'s foundational definition of individual fairness asks: "similar individuals should be treated similarly." But *who defines similarity?* The Fairlearn framework encodes human-group similarity as the relevant axis. The Ubuntu Decolonial Framework asks whether that encoding is itself a form of bias — privileging certain philosophical traditions over others. The speciesism proposal extends the same logic: privileging *Homo sapiens* over other sentient beings may be just as much an unexamined default as privileging white over Black, or male over female.

### 🎙️ Podcast Talking Points

1. **The customer question:** Fairlearn is Microsoft-backed. Microsoft has a major animal agriculture supply chain and a foundation funding animal welfare. Should a corporate funder's values determine what categories of harm a fairness tool measures? Is this different from any other funding influence?
2. **The granularity question:** If Fairlearn *should* measure speciesist bias, why not age, disability, or any other dimension the maintainers deem "out of scope"? Where is the principled line — and who draws it?
3. **The routing question:** When maintainers say "that's in scope for PyRIT, not Fairlearn," are they making a sound technical decision — or are they deflecting a normative challenge by passing it to a different tool with a different governance structure?
4. **The foundational question:** What does it mean about the field of algorithmic fairness that the most vibrant, productive debate in 2026 is about whether *non-human animals* belong in the framework — while the classic debates about human racial and gender bias remain unresolved? Does this suggest the field has a *scope* problem, not a *metric* problem?
5. **Ubuntu's answer:** The Ubuntu Decolonial Framework starts from the premise that fairness is a *relational* property of communities, not an individual-group comparison. From this perspective, the question "should fairness tools measure speciesism?" is slightly misplaced — the real question is *what kind of relational harm is occurring, and which communities recognize it?* A Western-liberal fairness metric might measure speciesist *bias*; an Ubuntu lens might ask whether the communities most dependent on animal livelihoods (e.g., pastoralists, smallholders in Sub-Saharan Africa) are even *heard* in the fairness framework's design.

---

## 📝 Contributing Debates

Have you found another heated thread in a fairness repo? Add it here!

| Issue | Repo | Core Question |
|---|---|---|
| [fairlearn #756](https://github.com/fairlearn/fairlearn/issues/756) | fairlearn/fairlearn | Should `MetricFrame` support metrics without `y_true`/`y_pred`? |
| [AIF360 #214](https://github.com/Trusted-AI/AIF360/pull/214) | Trusted-AI/AIF360 | Do "inequality indices" measure fairness or utility? |
| [fairlearn #1625](https://github.com/fairlearn/fairlearn/issues/1625) | fairlearn/fairlearn | Should fairness tools measure harm to non-human animals? |
| [AIF360 #97](https://github.com/Trusted-AI/AIF360/issues/97) | Trusted-AI/AIF360 | Can a tool "remove bias"? Should we reconsider the word "bias"? |

To add a new debate: fork this repo, append to `DEBATES.md`, and open a PR!