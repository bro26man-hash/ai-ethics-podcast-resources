# 🎙️ Episode Planner — AI Ethics & Social Justice Podcast

Episode outlines, hooks, key questions, and talking points for each episode in the series.

---

## Episode 1: "What Does Zero Mean?" — When the Math Says Fair But It Isn't

**Source issue:** [AIF360 #528](https://github.com/Trusted-AI/AIF360/issues/528)  
**Project:** [IBM AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Duration estimate:** 35-45 minutes

### The Hook
A volunteer spent 17 months trying to fix a documentation bug in the world's most-used fairness toolkit — and gave up. The bug means the tool can certify a model as "fair" when it's actually not. Who owns the definition of what "zero" means?

### Key Questions
1. What is `average_odds_difference` and why does the documentation say something it doesn't mean?
2. Can two ROC curves cross in a way that makes an unfair model look fair?
3. Who should fix documentation bugs in tools that courts and regulators depend on?
4. Is 17 months of no maintainer response normal for open-source?
5. Should fairness tools have a different standard for documentation than regular software?

### Talking Points
- The math is correct; the interpretation is wrong
- The error also appears in the text explainer tool
- IBM's own website repeats the error
- A volunteer offered a concrete fix with a four-row counterexample — nobody merged it
- The "zero" in a fairness metric isn't neutral — it's a political decision about what counts as fair

### Guest Suggestions
- The issue author (AndreFCruz) or the volunteer (Hanabi9248)
- A fairness researcher who can explain the difference between average odds and equalized odds
- A practitioner who has relied on AIF360 metrics in a real audit

### Follow-Up
- Listen to Episode 2 on intersectionality
- Read [FairMLBook, Chapter 4](https://fairmlbook.org/) on metric impossibilities

---

## Episode 2: "The 360 That Isn't" — When a Fairness Tool Can't Measure Intersectionality

**Source issue:** [AIF360 #558](https://github.com/Trusted-AI/AIF360/issues/558)  
**Project:** [IBM AI Fairness 360](https://github.com/Trusted-AI/AIF360)  
**Duration estimate:** 30-40 minutes

### The Hook
A fairness toolkit called itself "360" — but it can only measure fairness along one axis at a time. A volunteer offered to fix it. The maintainers haven't responded.

### Key Questions
1. What is intersectional analysis and why does it matter?
2. Can a single scalar capture the experience of Black women, disabled immigrants, or Indigenous transgender people?
3. Is a single fairness score regulatory-friendly or a form of erasure?
4. Who should pay for maintaining tools that courts depend on?

### Talking Points
- The scalar vs. breakdown tension is a political choice
- Regulatory frameworks typically ask for single scores per attribute
- But single scores can erase the worst-off groups
- The "360" in the name is aspirational — the math isn't there yet
- Same volunteer-maintenance pattern as Issue #528

### Follow-Up
- Watch Kimberlé Crenshaw's TED talk on intersectionality
- Read [Chouldechova (2017)](https://arxiv.org/abs/1703.00056) on impossibility theorems

---

## Episode 3: "Framework or Instrument?" — Who Should a Fairness Tool Serve?

**Source issue:** [FairLearn #756](https://github.com/fairlearn/fairlearn/issues/756)  
**Project:** [FairLearn — Microsoft](https://github.com/fairlearn/fairlearn)  
**Duration estimate:** 40-50 minutes

### The Hook
A FairLearn maintainer proposed making the API more flexible. Three other maintainers pushed back. Five years later, it's still unresolved. The question isn't just about code — it's about who the tool is for: researchers who need flexibility, or practitioners who need reliability?

### Key Questions
1. Should `MetricFrame` be a general-purpose framework or a specialized instrument?
2. Is API stability more important than flexibility?
3. Who decides: the maintainers, the users, or the communities being audited?
4. Why has this debate been running for 5+ years without resolution?
5. Does the disagreement among FairLearn's own maintainers tell us something about fairness itself?

### Talking Points
- The `**kwargs` vs. dictionary debate is about who can extend the tool
- The `metric` → `metrics` rename is about consistency vs. breakage
- keyword-only args would let you drop `y_true`/`y_pred` for non-standard metrics
- The maintainers themselves don't agree — this is a genuine open community question
- Novice users need simplicity; researchers need flexibility; auditors need reproducibility

### Follow-Up
- Read the full [74-comment thread](https://github.com/fairlearn/fairlearn/issues/756)
- See [FairLearn #1725](https://github.com/fairlearn/fairlearn/issues/1725) on missing-value contracts

---

## Episode 4: "Even the Auditors Need Auditing"

**Source issues:** [Aequitas #201](https://github.com/dssg/aequitas/issues/201), [Aequitas #209](https://github.com/dssg/aequitas/issues/209)  
**Project:** [Aequitas — University of Chicago](https://github.com/dssg/aequitas)  
**Duration estimate:** 30-40 minutes

### The Hook
A toolkit built for policymakers can't explain its own metrics. And it shipped breaking changes without telling users. If the fairness tool can't be trusted, can the audit?

### Key Questions
1. Should a tool labeled "for policymakers" need a "for policymakers" README?
2. Can you trust an audit whose tool version you can't verify?
3. Is a "Good First Issue" that's been open for 14 months still "good first"?

---

## Episode 5: "The Corporate Toolkit" — Whose Fairness Ships by Default?

**Source project:** [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)  
**Duration estimate:** 25-35 minutes

### The Hook
When a major IT company builds the fairness tooling, whose definition of "fair" becomes the default? Is enterprise fairness a public good or a proprietary standard?

### Key Questions
1. Who does the Infosys RAI toolkit serve: the audited or the auditor?
2. Is open-source fairness a transparency strategy or a genuine public good?
3. What would community-governed fairness metrics look like?

---

## 📋 Series Structure

| Episode | Theme | Repo | Issue |
|---|---|---|---|
| 1 | What Does Zero Mean? | AIF360 | #528 |
| 2 | The 360 That Isn't | AIF360 | #558 |
| 3 | Framework or Instrument? | FairLearn | #756 |
| 4 | Even the Auditors Need Auditing | Aequitas | #201, #209 |
| 5 | The Corporate Toolkit | Infosys RAI | — |

*Contributors: Add episode ideas to `PODCAST.md` — include the repo, issue, hook, and 2-3 key questions.*