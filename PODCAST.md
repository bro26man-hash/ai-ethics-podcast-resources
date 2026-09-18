# 🎙️ AI Ethics & Social Justice — Podcast episode notes

## Episode: Strict or Silent? The Missing-Value Debate in Fairness Toolkits

### Overview
This episode explores a live controversy in the FairLearn open-source project: whether fairness tools should raise errors or silently skip missing sensitive feature data. We connect this to broader questions about who owns the definition of "fairness" in practice.

### Key Resources
- [FairLearn Issue #1725](https://github.com/fairlearn/fairlearn/issues/1725) — The live debate
- [FairLearn PRs #1698 and #1713](https://github.com/fairlearn/fairlearn/pulls) — The PRs that created the inconsistency
- [AIF360 Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — Documentation accuracy in fairness metrics
- [Aequitas Issue #201](https://github.com/dssg/aequitas/issues/201) — Missing metric documentation in audit tools

### Discussion Prompts for Listeners
1. Should fairness tools be strict (raise errors) or permissive (handle gracefully) with missing data?
2. Who decides what "correct" behavior is — the maintainer, the contributor, or the user?
3. Can a tool that silently drops rows still produce fair outcomes?
