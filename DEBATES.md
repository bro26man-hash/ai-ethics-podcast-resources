# ⚖️ Live Debates from Open-Source Fairness Repos

Real controversies happening in real repos. Each entry is based on an actual open GitHub issue thread, verified as of September 2026.

---

## 🔴 Debate #1: The Average-Odds Documentation Bug — Who Owns the Definition of "Zero"?

**Source:** [Trusted-AI/AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Project:** [IBM AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Opened:** April 23, 2024  |  **Last updated:** September 11, 2026  |  **Status:** Open  |  **👍 Reactions:** 2  |  **💬 Comments:** 1  
**Labels:** None (no maintainer has categorized or assigned it)  
**Issue author:** [AndreFCruz](https://github.com/AndreFCruz)  
**Volunteer responder:** [Hanabi9248](https://github.com/Hanabi9248) (September 11, 2026)

### The Claim

**AndreFCruz**, a developer and external contributor, filed an issue arguing that the `average_odds_difference` metric in AIF360 is **mathematically misdocumented**. The [API docstring](https://aif360.readthedocs.io/en/stable/modules/generated/aif360.metrics.ClassificationMetric.html) states:

> *"A value of 0 indicates equality of odds."

Andre says this is **false**. He provides a visualization (attached to the issue) showing that there exist pairs of ROC curves where `average_odds_difference = 0` but **equalized odds do not hold**. The error, he points out, is not just in the AIF360 documentation — it also appears on **IBM's own fairness metrics webpage**.

### The Mathematics Behind the Bug

The `average_odds_difference` is defined as the average of the absolute differences in False Positive Rates (FPR) and False Negative Rates (FNR) between groups. Andre's key insight: **two ROC curves can cross** in a way that produces an average odds difference of zero — meaning the FPR difference and FNR difference cancel each other out — even though neither FPR parity nor FNR parity holds individually. Equalized odds requires *both* TPR parity *and* FPR parity simultaneously, which is strictly stronger than average odds difference = 0.

He demonstrates this with a four-row example where:
- `average_odds_difference = 0` (the metric says "fair")
- `average_abs_odds_difference = 1` (the absolute version says "unfair")
- `equalized_odds_difference = 1` (the correct metric says "unfair")

This means a practitioner relying on the documented metric could **certify a model as fair** when it is demonstrably unfair.

### Why This Matters

The "average odds difference" metric is not an obscure corner case. It is one of the standard metrics shipped in the most widely used fairness toolkit in the world. If the documentation tells practitioners that zero means equality of odds — and that is not actually true — then:

1. **Audits can pass when they shouldn't.** A model could score 0 on average odds difference and still violate equalized odds, leading to a false sense of fairness.
2. **The error propagates.** IBM's website, tutorial notebooks, and downstream documentation all repeat the claim. Anyone learning fairness from AIF360 inherits the mistake.
3. **The definition of fairness is being silently distorted.** When a toolkit's docs are wrong about what a metric means, the toolkit shapes how an entire field thinks about fairness.
4. **The explainer is also wrong.** The `MetricTextExplainer` — the tool that generates human-readable explanations of fairness results — contains the same error. If the explainer says "this model achieves equalized odds" when it doesn't, the mismatch is not academic. It's operational.

### The Response — Or Lack Thereof

After **17 months** with no maintainer response, **Hanabi9248** (a volunteer) commented on September 11, 2026:

> *"Could you assign this to me? The cancellation also appears in MetricTextExplainer and its JSON output. I have a focused correction for both API docstrings and the explanations, with a four-row example where average_odds_difference is 0 while average_abs_odds_difference and equalized_odds_difference are both 1. The metric formulas remain unchanged."

Key observations about Hanabi9248's offer:
- The **metric formulas are mathematically correct** — the bug is purely in the interpretation/documentation, not the computation.
- The error appears in **two places**: the API docstring AND the automated text explainer.
- A concrete **four-row counterexample** already exists.
- Yet after 6+ months since the volunteer offered, the issue remains **unassigned and unmerged**.

This is a stunning admission in its own right: the error is **not just in the docs** — it's also in the `MetricTextExplainer`, the tool that generates human-readable explanations of fairness results.

### The Deeper Question: Who Decides What Zero Means?

This issue is really about **epistemic authority** in open-source fairness tools:

- **Is a metric what its formula says it is?** The formulas aren't changing — Hanabi9248 confirmed that. The math is fine. The problem is the *interpretation* attached to the result.
- **Does the documentation create reality?** If 1,000 practitioners read "A value of 0 indicates equality of odds" and trust it, does that make it true in practice — even if it's mathematically wrong?
- **Who bears the cost of a doc bug?** AndreFCruz filed it. Hanabi9248 offered to fix it. But no AIF360 maintainer has merged the correction in 17 months. The people closest to the fix are not the people with commit access.
- **Should fairness tools be more humble?** Maybe the real issue is that any single scalar claiming to capture "equality of odds" deserves a warning label — not a docstring that pretends it's definitive.
- **Is the "bytecode of fairness" trustworthy?** If the documentation layer is wrong, can you trust any output from the toolkit? The code may be right, but the interface between human and machine is lying.

### The Broader AIF360 Maintenance Picture

This documentation bug is not an isolated case. During research, we found several other open issues in the AIF360 repo that paint a picture of a project under strain:

- **[#548 — Website is down](https://github.com/Trusted-AI/AIF360/issues/548):** Open since February 2025 with 7 comments. The official documentation website has been unavailable, making it harder for users to access metric definitions. A dead website + a misleading docstring = a double documentation failure.
- **[#558 — Extend Empirical Differential Fairness metric](https://github.com/Trusted-AI/AIF360/issues/558):** Open since January 2026. A contributor wants to *add* a new metric for intersectional analysis, but the existing metrics still have documentation errors. This raises the question: should you add new metrics before fixing the old ones?
- **[#526 — Memory Management Issue in ClassificationMetric](https://github.com/Trusted-AI/AIF360/issues/526):** Open since April 2024. A core class has performance problems.
- **[#379 — Add ThresholdOptimizer to sklearn-compatible post-processing](https://github.com/Trusted-AI/AIF360/issues/379):** Open since September 2022. A basic usability enhancement, labeled "good first issue."

The pattern: **documentation errors, infrastructure decay, and performance issues** all coexist. Volunteer corrections are welcome but unassigned.

### Two Competing Narratives

**Narrative A — "It's just documentation"**
The metrics work correctly. The formulas are sound. A docstring error is a minor annoyance, not a fundamental flaw. The volunteer can fix it whenever the maintainer gets to it. Fairness research is fast-moving; the underlying theory (Hardt et al., 2016) is well-established.

**Narrative B — "Documentation IS the tool"**
For most practitioners, the docstring IS the fairness definition. They don't read the paper (Hardt et al., 2016). They don't derive the formula. They read the documentation and trust it. When the documentation says "zero means equality of odds," that becomes the working definition for thousands of audits, maybe millions of predictions. A doc bug isn't minor — it's a **silent failing** that corrupts the entire audit pipeline.

### Discussion Prompts for the Episode

1. Should fairness toolkits be required to publish formal verification of their metric definitions — the way cryptographic libraries publish formal proofs?
2. Who should maintain the "official" definitions of fairness metrics — a single org, a consortium, or the community?
3. If a documentation error in a fairness toolkit leads to a biased decision in a court, who bears liability?
4. Is 17+ months of unmaintained documentation a symptom of the "research-to-production gap" in AI ethics?
5. When two tools (AIF360 vs. FairLearn) define the same metric differently, which definition should a regulator adopt?
6. Is a volunteer-submitted fix with no maintainer merge pathway actually authoritative?
7. Should fairness tool maintainers be required to respond to issues within a certain timeframe? What would that look like?
8. If the documentation is wrong but the code is right, should you trust the tool? What if the documentation is right but the code is wrong?
9. Should the AIF360 toolkit Display a warning whenever `average_odds_difference` is near zero, prompting users to check `equalized_odds_difference` as well?
10. Is the "pull request" model of open-source sufficient for safety-critical tools, or do fairness metrics need a review process like clinical trials?

---

## 🔴 Debate #2: The Intersectional Analysis Gap — Should Fairness Tools Return a Scalar or a Breakdown?

**Source:** [Trusted-AI/AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558)  
**Project:** [IBM AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Opened:** January 21, 2026  |  **Last updated:** September 16, 2026  |  **Status:** Open  
**Issue author:** [jetverbeek](https://github.com/jetverbeek)  
**Volunteer responder:** [Hanabi9248](https://github.com/Hanabi9248) (September 16, 2026)

### The Question

The current implementation of the Empirical Differential Fairness (EDF) metric only returns a **single scalar value** summarizing fairness across protected attributes. But for intersectional analysis, a single number hides the story: which specific combinations of attribute groups contribute most to the maximum log-ratio?

jetverbeek asked for the metric to return:
- The pair(s) of attribute groups that produce the max log ratio
- A breakdown of all/top X group combinations and their respective log-ratios

### The Volunteer's Proposal

After two months, **Hanabi9248** responded on September 16, 2026:

> *"I would keep smoothed_empirical_differential_fairness() returning a scalar and add a separate method for the group-pair breakdown, including the pairs attaining the maximum and an optional top-k limit. The calculations would use the same instance weights and Dirichlet smoothing as the existing metric, with tests for ties and multiple protected attributes. Please let me know if you prefer an optional details argument on the existing method instead."

Key observations:
- Hanabi9248 proposed a **backward-compatible** design — the scalar return stays, a new method adds the breakdown
- The proposal preserves the existing mathematical framework (Dirichlet smoothing, instance weights)
- It includes **tests for edge cases** (ties, multiple protected attributes)
- Yet again, the issue remains **unassigned and unmerged**

### The Two Sides

**Side A — Single scalar is enough for most use cases.** Regulatory frameworks (EU AI Act, FTC guidance) typically ask for a single fairness score per protected attribute. A scalar is simpler to report, simpler to compare, and simpler to regulate against.

**Side B — Intersectional analysis is essential for real fairness.** A single scalar can hide severe disparities for specific subpopulations. Black women, disabled immigrants, Indigenous transgender people — these groups exist at the intersection of multiple protected attributes, and a scalar that averages across all of them can erase their specific experience of discrimination.

### Why It Matters

1. **The scalar vs. breakdown tension is a political choice.** A single "fairness score" simplifies regulation but can erase the communities that need the most protection.
2. **Who decides the granularity of fairness reporting?** Regulators, practitioners, or the communities being audited? Currently, the answer is: whoever maintains the toolkit.
3. **The volunteer-maintenance pattern, again.** Both AIF360 debates (#528 and #558) share the same pattern: a volunteer offers a fix or enhancement, but the issue sits unassigned. Is IBM Research delegating fairness standardization to volunteers? And should we trust them to do so?
4. **The "360" in AIF360.** If the toolkit can't tell you which specific group combinations are most disadvantaged, is it really measuring fairness along 360 degrees? Or is it only measuring fairness along single axes?

### Discussion Prompts

1. Should fairness tools be required to support intersectional analysis by default, or is a single scalar acceptable for regulatory contexts?
2. If a single scalar can mask the worst-off group, is it ethical to ship that as the default output?
3. Who should pay for the maintenance of fairness tools that courts and regulators depend on?
4. If a volunteer can write the code but can't merge it, is the tool truly "open source" — or is it "open to the community but closed to the community"?
5. Should the name "AI Fairness 360" change if it can't actually measure fairness along intersecting axes?

---

## 🔴 Debate #3: The MetricFrame API Design Debate — General-Purpose Framework or Specialized Instrument?

**Source:** [fairlearn/fairlearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Opened:** April 22, 2021  |  **Last updated:** September 1, 2021 (+ subsequent comments through 2026)  |  **Status:** Open  |  **💬 Comments:** 74  
**Labels:** API  
**Issue author:** [MiroDudik](https://github.com/MiroDudik) (FairLearn co-creator, Microsoft Research)

### The Core Question

FairLearn's `MetricFrame` class — its flagship API for computing disaggregated fairness metrics — only supports metrics with the signature `metric(y_true, y_pred)`. But fairness metrics are NOT all classification metrics. The issue, filed by MiroDudik (one of FairLearn's creators), asks:

> *Should MetricFrame support metrics that don't require y_true and y_pred?*

This is not a minor API polish request. It's a **fundamental disagreement about what a fairness tool should be**.

### The Two-Step Proposal

MiroDudik proposed two changes:

**Step 1 — Make all arguments keyword-only:**
```python
# Current API:
MetricFrame(metric, y_true, y_pred, *, sensitive_features, control_features=None, sample_params=None)

# Proposed:
MetricFrame(*, metrics, y_true=None, y_pred=None, sensitive_features, control_features=None, sample_params=None)
```

Making `y_true` and `y_pred` optional would allow metrics that don't need them — like dataset-only metrics (demographic parity) or streaming metrics.

**Step 2 — Allow flexible sample parameters:**
```python
MetricFrame(*, metrics, sensitive_features, control_features=None, sample_params=None, **shared_sample_params)
```

This would let any metric receive any parameter it needs — `y_score`, `costs`, `actions_taken`, `rewards`, `propensities` — without needing wrapper functions or dummy variables.

### The Resistance

The debate that followed revealed a deep fault line in the fairness tooling community:

**riedgar-ms** (MetricFrame's original author): *"I would prefer to keep MetricFrame as it is — doing a fairly simple thing well — and work out the best API for a new use case from a clean sheet."*

**hildeweerts**: *"I am afraid that trying to fit all different kinds of learning tasks into one MetricFrame will become extremely confusing for novice users."*

**romanlutz**: *"If we move to `**shared_sample_params` then we better have extremely good documentation. Reading this in an API documentation I would have no clue what to do with that."*

**riedgar-ms again**: *"I'm not keen on accepting `**kwargs` in a function signature. It means that if we ever want to add new arguments, we're going to break someone."*

### The Concrete Scenarios

MiroDudik responded with four real-world scenarios that the current API can't handle cleanly:

1. **Classification + scoring metrics together**: You want to evaluate both accuracy AND ROC-AUC on the same disaggregated dataset. Currently you need lambda wrappers.
2. **Multiple models in one frame**: Comparing 3 models' accuracy across sensitive groups. Currently you need different lambda wrappers per model.
3. **Streaming metrics**: Online fairness monitoring where data arrives incrementally. `y_true` and `y_pred` start as empty lists.
4. **Metrics beyond classification**: Cost-sensitive learning (parameters: `costs`, `y_pred`) and contextual bandits (parameters: `actions_taken`, `rewards`, `propensities`). Currently you need dummy `y_true`/`y_pred` just to pass the right parameters.

### The Compromise — and Why It's Still Unresolved

After 74 comments, the closest thing to consensus was:

- Make arguments keyword-only (with deprecation warnings)
- Add `shared_sample_params` as a **dictionary** (not `**kwargs`)
- Keep `y_true` and `y_pred` as required (riedgar-ms' preferred position)

But even this modest compromise has sat unresolved for **5+ years**. The issue was last updated in 2026.

### Why This Debate Matters for the Podcast

This is not just about Python syntax. The API design question **encodes a philosophical position** about what fairness tooling is for:

1. **General-purpose vs. specialized**: Should a fairness toolkit try to support every possible evaluation scenario (bandits, streaming, cost-sensitive, NLP, vision), or should it do classification/regression metrics really well and let other tools handle the rest?

2. **Who does the tool serve?** If you're a researcher working on Fairness in reinforcement learning, the current MetricFrame is useless to you. You have to write wrapper functions that hide the fact that you're not doing classification. The API assumes the user is a classification practitioner.

3. **The elegance vs. flexibility tradeoff**: riedgar-ms and hildeweerts argue for simplicity — a clean API that novices can understand. MiroDudik argues for flexibility — a general API that researchers can extend. This is the same tension that runs through all of software design, but in fairness tooling it has real consequences: if the tool can't express your fairness question, your question doesn't exist.

4. **The maintainer gap**: MiroDudik is a FairLearn co-creator. riedgar-ms is the original author. Neither is a current maintainer. The debate has been running for 5 years without resolution. Who decides what FairLearn becomes?

5. **The "**kwargs" question as a fairness question**: riedgar-ms's objection to `**kwargs` — "if we ever want to add new arguments, we're going to break someone" — is really about **backward compatibility as a form of stability**. If fairness tools keep changing their APIs, can audit results ever be reproducible? This is the same concern as Aequitas' versioning issue (Debate #5), but from the opposite direction: FairLearn is too conservative, Aequitas is too aggressive.

### Two Competing Narratives

**Narrative A — "Keep it simple, do one thing well"**
FairLearn's core use case is classification and regression fairness. The API is clean, intuitive, and well-documented. Expanding it to support bandits, streaming, and cost-sensitive learning would make it confusing for the majority of users who just want to check demographic parity and equalized odds. Other tools can handle those cases.

**Narrative B — "A fairness tool should be able to express any fairness question"**
If FairLearn can't evaluate fairness in contextual bandits or cost-sensitive settings, it's implicitly saying those settings don't matter. The real world of fairness isn't just binary classification — it's lending decisions under partial observability, healthcare resource allocation, content recommendation. A generality gap in the tool is a justice gap.

### Discussion Prompts for the Episode

1. Should a fairness tool be a general-purpose metrics framework or a specialized instrument? What's the right balance?
2. Does the API design of a fairness tool encode a philosophical assumption about what fairness is? What assumptions does MetricFrame's `y_true, y_pred` signature encode?
3. If a fairness tool can't evaluate your specific use case (bandits, streaming, NLP), is fairness "not applicable" to your domain — or is the tool insufficient?
4. How should open-source fairness projects balance backward compatibility with innovation? Should breaking changes ever be allowed?
5. Who should decide the direction of an open-source fairness tool — the original authors, current maintainers, the community, or a governance board?
6. Is 5 years without resolving a core API question a sign of healthy deliberation or governance failure?
7. Should fairness tools have a formal specification (like an RFC or an API contract) before implementation, the way HTTP and TCP do?
8. If a researcher can't express their fairness question in the tool's API, does that question effectively not exist in the field?
9. How do we prevent fairness tooling from being designed by and for a single subfield (classification) while ignoring other domains?
10. Is the "extension" pattern (wrappers, lambda functions, custom classes) sufficient, or should the core API be redesigned?

---

## 🔴 Debate #4: The Metrics Documentation Gap — Even the Auditors Need Auditing

**Source:** [dssg/aequitas Issue #201](https://github.com/dssg/aequitas/issues/201)  
**Project:** [Aequitas — University of Chicago](https://github.com/dssg/aequitas)  
**Opened:** July 23, 2024  |  **Last updated:** September 29, 2025  |  **Status:** Open  
**Labels:** Good First Issue, Base, Documentation  
**Assignees:** reluzita, VinayakPaka, Vijaygaurav2004

### The Claim

Aequitas, the toolkit *built for policymakers and data scientists*, doesn't have a single clear page explaining what each of its fairness metrics actually measures. [Issue #201](https://github.com/dssg/aequitas/issues/201) asks for a dedicated README or documentation page that summarizes all available fairness metrics — yet it has been open for over a year with three contributors assigned and no clear resolution.

### Why This Matters

This is a **meta-documentation problem**: the tool that's supposed to help people understand fairness doesn't even fully document its own fairness metrics. If Aequitas can't clearly explain what `tpr`, `fpr`, and `pprev` mean in a fairness context, how can a policymaker trust the audit output? The irony is profound: *the fairness toolkit needs a fairness audit of its own documentation.*

### Discussion Prompts

1. Should a fairness toolkit be required to document its metrics in plain language — not just math?
2. If a tool labeled "for policymakers" can't explain its own metrics, what does that say about who the tool is really for?
3. Is a "Good First Issue" that's been open for 14 months still "good first" — or has it become a symbol of something else?
4. Should fairness documentation be subject to the same kind of peer review as the metrics themselves?

---

## 🔴 Debate #5: The Versioning Trust Gap — When Breaking Changes Break Trust

**Source:** [dssg/aequitas Issue #209](https://github.com/dssg/aequitas/issues/209)  
**Project:** [Aequitas — University of Chicago](https://github.com/dssg/aequitas)  
**Opened:** December 14, 2024  |  **Status:** Open  
**Labels:** Bug, CLI

### The Claim

A user reported that the Aequitas team released new versions with **breaking changes** — changes to the CLI that would cause existing scripts and workflows to fail — without bumping the version number appropriately. For a tool used in fairness audits, this is not just an inconvenience. It's an **integrity problem**.

### Why This Matters

Fairness audit results are used as **evidence** — in regulatory compliance, in legal proceedings, in policy decisions. If the tool versions are not clearly tracked, you cannot reproduce an audit. If you cannot reproduce an audit, the audit is not evidence. It's just an opinion.

The versioning question is really: **Can you trust an audit tool that doesn't tell you what changed?**

### Discussion Prompts

1. Should fairness tools follow Semantic Versioning strictly — or is a looser convention acceptable?
2. If a fairness audit was run with version 1.0 and the tool ships version 2.0 with breaking changes, is the old audit still valid?
3. Should audit reports be required to include the exact tool version, or is that overkill?
4. Who is responsible when a version change causes a previously "fair" model to suddenly appear "unfair" — the tool or the auditor?

---

## 🔴 Debate #6: The Missing-Value Contract — What Should a Fairness Tool Do When Data Is Incomplete?

**Source:** [fairlearn/fairlearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Opened:** September 9, 2026  |  **Last updated:** September 16, 2026  |  **Status:** Open  |  **💬 Comments:** 3  
**Issue author:** [aiedwardyi](https://github.com/aiedwardyi)

### The Question

`MetricFrame` and `plot_roc_curve_by_group` handle missing sensitive feature values differently. When the data doesn't include race, gender, or another protected attribute, should the tool:

- **Option A — Fail loudly** and tell the user "you can't compute fairness without knowing who's in the group"?
- **Option B — Proceed silently** and compute what it can, perhaps with a warning?
- **Option C — Impute or estimate** the missing values using statistical methods?

The disagreement between MetricFrame and the ROC curve plotting function suggests that Fairlearn's own maintainers haven't agreed on the answer.

### Why It Matters

This question reveals a fundamental tension in fairness tooling:

1. **Strictness vs. usability.** A strict tool that refuses to run without complete data is more principled but less useful in the real world, where data is often messy.
2. **Silence vs. transparency.** If a tool proceeds silently with missing data, the user might not realize the fairness assessment is incomplete. If it warns, the user might ignore the warning.
3. **Who bears the risk?** If a tool imputes missing values and gets it wrong, whose responsibility is the error? The tool developer? The data scientist? The regulator who accepted the tool's output?
4. **The philosophical question underneath.** A fairness tool requires sensitive data to detect bias. But in many contexts (hiring, lending, healthcare), collecting sensitive data is itself controversial. The tool's approach to missing data reveals its assumptions about whether fairness can be assessed without explicit group membership — and that's a political question, not a technical one.

### Discussion Prompts

1. Should fairness tools be strict or silent when sensitive data is missing?
2. Is it ethical for a fairness tool to produce an incomplete assessment without clearly flagging the gap?
3. Can you measure fairness without knowing who's being measured — and should you try?
4. Who decides what constitutes "missing" data? Is race missing because it wasn't collected, or because it was deliberately excluded?

---

## 🔴 Debate #7: The Corporate Fairness Question — Whose Definition Ships by Default?

**Source:** [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)  
**Project:** Enterprise Responsible AI Platform  
**Status:** Open-source, actively maintained

### The Question

When a major IT services company builds a "Responsible AI" toolkit, whose definition of fairness becomes the default? The toolkit covers fairness, safety, security, explainability, and hallucination detection — but it also serves the company's commercial interests. Does the toolkit optimize for the fairest outcome, or for the most marketable one?

### Why It Matters

This is the question that doesn't appear in issue threads but pervades every corporate open-source project:

1. **Corporate vs. community governance.** When Infosys decides which fairness metrics to include, which to prioritize, and which to deprecate, whose interests guide those decisions?
2. **The "responsible" in Responsible AI.** Is the toolkit responsible to the people being audited, or to the organizations doing the auditing?
3. **Open source as transparency vs. open source as strategy.** Is the toolkit open-source to enable scrutiny, or to establish Infosys's fairness definitions as industry standards?
4. **The regulatory capture risk.** If regulators adopt a corporate toolkit's metrics as their reference, the corporation effectively writes the rules it benefits from.

### Discussion Prompts

1. Should fairness metrics be governed by an independent consortium rather than a single company?
2. Is it possible for a corporate-built tool to be genuinely "fair" — or does the power asymmetry inherently bias the output?
3. Should regulated industries be required to use independently governed tools rather than vendor-provided ones?
4. What would "community-governed" fairness metrics look like — and could it compete with enterprise-grade tooling?

---

## 🔗 Quick Links to All Live Debates

| # | Debate | Repo | Issue | Open Since | Status |
|---|---|---|---|---|---|
| 1 | What does "zero" mean? | AIF360 | [#528](https://github.com/Trusted-AI/AIF360/issues/528) | April 2024 | Open, unmaintained |
| 2 | Scalar vs. intersectional breakdown | AIF360 | [#558](https://github.com/Trusted-AI/AIF360/issues/558) | January 2026 | Open, volunteer waiting |
| 3 | General-purpose vs. specialized API | FairLearn | [#756](https://github.com/fairlearn/fairlearn/issues/756) | April 2021 | Open, 5+ years unresolved |
| 4 | Metrics documentation gap | Aequitas | [#201](https://github.com/dssg/aequitas/issues/201) | July 2024 | Open, 3 assignees |
| 5 | Versioning trust gap | Aequitas | [#209](https://github.com/dssg/aequitas/issues/209) | December 2024 | Open |
| 6 | Missing-value contract | FairLearn | [#1725](https://github.com/fairlearn/fairlearn/issues/1725) | September 2026 | Open, 3 comments |
| 7 | Whose fairness definition? | Infosys RAI | — | — | Open-source, corporate |

---

## ✍️ How to Contribute a Debate

Found a fairness controversy in an open-source issue thread? Contribute:

1. Link the issue/PR
2. Summarize the two (or more) sides
3. Explain *why* the debate matters beyond the repo
4. Add discussion prompts for podcast listeners

Format: follow the structure above — **The Claim → Why It Matters → The Response → The Deeper Question → Discussion Prompts**

---

*Debates are sourced from real GitHub issue threads. All issues verified as open in September 2026. The summary represents a particular perspective; listener and contributor counterarguments are welcome — open an issue or submit a PR.*