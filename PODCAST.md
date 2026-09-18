# 🎙️ Episode Planner — Technology & Social Justice Podcast

## Featured Episode: "Who Does the Tool Serve? — The FairLearn MetricFrame Debate"

**Source Issue:** [fairlearn/fairlearn #756](https://github.com/fairlearn/fairlearn/issues/756)
**Related Issue:** [Trusted-AI/AIF360 #528](https://github.com/Trusted-AI/AIF360/issues/528)
**Projects:** FairLearn (2,286 ⭐), AIF360 (2,866 ⭐)

### The Hook

A fairness toolkit maintainer wants to make the tool more powerful. Other maintainers want to keep it simple. 74 comments later, it's still unresolved. And the answer determines whose fairness gets measured — and whose gets invisible.

### Key Questions for the Episode

1. **The API as values statement** — When FairLearn's `MetricFrame` only supports `metric(y_true, y_pred)`, what kinds of fairness problems become invisible?
2. **The novice-expert tradeoff** — Is simplicity a form of inclusion, or a form of exclusion?
3. **Maintainer power** — Who gets to decide what a fairness tool can do? Should users have a say?
4. **The regulation gap** — EU AI Act requires fairness assessments across diverse use cases. Can a tool that only handles classification meet that requirement?

### Talking Points

- MiroDudik's Alternative A vs. Alternative B (see DEBATES.md for full code examples)
- The `shared_sample_params` compromise — what it is, and what it means
- Why `**kwargs` scares riedgar-ms (and whether he's right)
- The parallel with AIF360's documentation bug — both are about who controls the definition of fairness
- The missing voice: neither issue thread has a "community user" perspective — only maintainers

### Guest Suggestions

- A FairLearn maintainer (reach out via Slack/issue)
- A practitioner who uses MetricFrame in production
- A researcher working on fairness in non-classification settings (bandits, RL, causal inference)
- A regulator or policyworker using fairness toolkits for compliance

### Segment Structure

1. **Cold open** — Read MiroDudik's opening post from Issue #756
2. **The debate** — Walk through the two sides, using real code examples
3. **The deeper question** — Who does the tool serve, and who decides?
4. **The parallel** — Connect to AIF360's documentation bug (Issue #528)
5. **Community discussion** — Address issues #1 and #2 in this repo (see below)

### Follow-Up Actions

- [ ] Listeners: Add your perspective to the Discussion Issue in this repo
- [ ] Contributors: Submit PRs with additional fairness projects for RESOURCES.md
- [ ] Everyone: Check out the live issue threads and add your voice

---

## Upcoming Episodes (Planned)

1. **"Zero Says What? — The AIF360 Documentation Bug"** — When the most-cited fairness toolkit ships a math error in its docstring
2. **"The Impossibility Triangle"** — Why you can't satisfy all fairness metrics simultaneously, and whose rights win
3. **"48% vs 42% — The SHAP Measurement Problem"** — How a denominator choice becomes a rhetorical decision
4. **"The Intersectional Analysis Gap"** — Should fairness tools return a single scalar or a breakdown?
5. **"The Generative Fairness Frontier"** — When 'bias' expands to include hallucinations

---

## Episode Archive

| # | Title | Source | Status |
|---|---|---|---|
| 1 | Who Does the Tool Serve? | FairLearn #756 | 🎙️ Recording |
| 2 | Zero Says What? | AIF360 #528 | 📝 Pre-production |
| 3 | The Impossibility Triangle | Fair-Code #665 | 📝 Research |
| 4 | 48% vs 42% | Fair-Code #672 | 📝 Research |
| 5 | The Intersectional Analysis Gap | AIF360 #558 | 📝 Research |
| 6 | The Generative Fairness Frontier | Infosys RAI | 📝 Research |
