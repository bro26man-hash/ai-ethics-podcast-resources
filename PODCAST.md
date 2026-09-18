# 🎙️ AI Ethics & Social Justice — Podcast Episode Planner

## Episode 1: What Does "Zero" Mean in a Fairness Metric?

### The Hook
The most-cited fairness toolkit in the world (IBM AIF360, 2,866 GitHub stars) ships a metric docstring that says "a value of 0 indicates equality of odds." A volunteer proved that's mathematically wrong. After 17 months, no maintainer has merged the fix.

### The Key Question
**Who owns the definition of fairness — the researchers who publish the metrics, the companies that ship the tools, or the communities who live with the decisions those tools produce?**

### The Source Material
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — The documentation bug
- [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — The intersectional analysis gap
- [AIF360 Issue #548](https://github.com/Trusted-AI/AIF360/issues/548) — The website is down
- [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — Who does the tool serve?
- [FairLearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725) — The missing-value contract

### The Guests We'd Love
- A maintainer of AIF360 or FairLearn
- A regulator who references these tools (FTC, EU AI Act office)
- A community organizer who's been affected by algorithmic decisions
- Hanabi9248 — the volunteer who tried to fix the bug

### The Talking Points
1. The average_odds_difference docstring says "0 = equality of odds" — but that's false. What happens when the tool's documentation lies by omission?
2. A volunteer offered a fix. No maintainer merged it for 17 months. Is "open source" a promise or a suggestion?
3. Two mainstream toolkits (AIF360 vs. FairLearn) define overlapping metrics differently. Which one should a regulator adopt?
4. The intersectional analysis gap: most tools return a single scalar per protected attribute. But race + gender, disability + age — these intersections are where the worst abuses happen.
5. Should fairness tool maintainers be required to respond to issues? What would that look like?

### The Listener Action
- Read the issue thread and leave a comment
- Try computing `average_odds_difference` vs. `equalized_odds_difference` on a real dataset
- Open a discussion in this repo with your take

---

## Episode 2: Who Does the Tool Serve?

### The Hook
FairLearn's MetricFrame API only works with metrics that have the signature `metric(y_true, y_pred)`. But contextual bandits, streaming data, and cost-sensitive learning don't fit that shape. The maintainer wants to generalize. The co-maintainer says it'll confuse novice users.

### The Key Question
**Should a fairness tool be a general-purpose framework or a specialized instrument — and who gets to decide?**

### The Source Material
- [FairLearn Issue #756](https://github.com/fairlearn/fairlearn/issues/756) — 74 comments, 5 months, unresolved
- [AIF360 Issue #558](https://github.com/Trusted-AI/AIF360/issues/558) — Scalar vs. breakdown

### The Talking Points
1. The 74-comment thread reveals a deep divide between simplicity and generality
2. API design is a political act — every choice excludes someone
3. The maintenance bottleneck: when maintainers go silent, who fills the gap?
4. The downstream harm: if a regulator references a tool's documentation, the API design becomes a policy outcome

---

*Contributors: Add episode ideas to this file or open an issue with the `episode-suggestion` label.*