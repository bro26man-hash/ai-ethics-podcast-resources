# 🎙️ Episode Planner — AI Ethics & Social Justice Podcast

## Current Episode: "The API Is the Argument — What Fairness Tools Can't Agree On"

### Hook

The most popular fairness toolkit in the world can't even agree on what arguments its own API should accept. A 74-comment issue, open for 5+ years, reveals that fairness tool design is not a technical question — it's a philosophical one about who gets to define fairness.

### Key Questions

1. Should a fairness tool be a general-purpose framework or a specialized instrument?
2. Does the API design of a fairness tool encode a philosophical assumption about what fairness is?
3. If a fairness tool can't express your specific use case, does your question effectively not exist?
4. Who should decide what an open-source fairness tool becomes — original authors, current maintainers, or the community?
5. How do we prevent fairness tooling from being designed by and for a single subfield?

### Talking Points

- FairLearn's `MetricFrame` only supports `metric(y_true, y_pred)` — what about bandits, streaming metrics, cost-sensitive learning?
- The creator (MiroDudik) wants to make arguments keyword-only and add flexible sample parameters
- The original author (riedgar-ms) wants to keep it simple — "do one thing well"
- The compromise (keyword-only args + shared_sample_params dict) has sat unresolved for 5+ years
- This isn't just about Python syntax — it's about who the tool serves: researchers, practitioners, or the communities being audited?

### Sources
- [FairLearn Issue #756 — MetricFrame should support metrics that don't require y_true and y_pred](https://github.com/fairlearn/fairlearn/issues/756)
- [FairLearn documentation](https://fairlearn.readthedocs.io/en/latest/)
- [FairLearn tutorials](https://github.com/fairlearn/fairlearn/tree/main/examples/notebooks)

### Guest Suggestions
- MiroDudik (FairLearn co-creator, filed Issue #756)
- riedgar-ms (MetricFrame original author)
- A fairness practitioner who's hit the MetricFrame limitations in production
- An ethicist who studies the politics of metric design

### Episode Arc

1. **Cold open**: Read the first comment from Issue #756 — the calm, technical proposal that started a 5-year debate
2. **The setup**: Explain what MetricFrame does and why it matters
3. **The breakdown**: Walk through the two competing visions for fairness tooling
4. **The deeper question**: What does it mean when a tool's design excludes certain fairness questions?
5. **The connect**: Link to other debates (AIF360's documentation bug, Aequitas's versioning gap)
6. **The call to action**: What side are you on? Open an issue in this repo to share your view

### Pre-Listening

- [RESOURCES.md](RESOURCES.md) — All the fairness toolkits
- [DEBATES.md](DEBATES.md) — All the live controversies

### Post-Listening

- Open an issue with your counterargument or perspective
- Add your own fairness toolkit to RESOURCES.md
- Nominate a debate from another repo

---

## upcoming Episodes (Draft)

- *"What Does Zero Mean?"* — AIF360's average_odds_difference documentation bug (Issue #528)
- *"The 360 That Isn't"* — Intersectional analysis gaps in AIF360 (Issue #558)
- *"Even the Auditors Need Auditing"* — Aequitas's own documentation gap (Issue #201)
- *"The Corporate Toolkit"* — Whose fairness definition ships by default in corporate open-source?

---

*Contributors: Add your episode ideas, guest suggestions, and talking points. Open an issue with label `episode-suggestion` to propose topics.*