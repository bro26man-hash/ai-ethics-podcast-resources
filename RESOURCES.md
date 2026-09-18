# 📚 Curated Open-Source Fairness & Bias-Auditing Projects

This page links the active open-source projects we track on the podcast. Each entry includes a short description, why it matters for the social-justice angle, and where to find live debates (issue threads, PR discussions).

---

## 1. [IBM AI Fairness 360 (AIF360)](https://github.com/Trusted-AI/AIF360)

| | |
|---|---|
| **Owner** | Trusted-AI (IBM) |
| **Stars** | ⭐ 2,866 |
| **Language** | Python (also R) |
| **License** | Apache-2.0 |
| **Last active** | Updated September 2026 |

**What it is:** An extensible open-source toolkit containing a comprehensive set of fairness metrics for datasets and models, explanations for those metrics, and algorithms to mitigate bias throughout the AI lifecycle. Covers 15+ bias-mitigation algorithms (pre-processing, in-processing, post-processing) and metrics like Demographic Parity, Equalized Odds, Predictive Parity, Average Odds, and Rich Subgroup Fairness.

**Why it matters for social justice:** AIF360 was designed to translate algorithmic fairness research into real-world practice across finance, hiring, healthcare, and education. Its metrics are referenced in policy discussions around the EU AI Act and NIST AI RMF. When a metric is mislabeled or misunderstood in this toolkit, it can mislead practitioners who build high-stakes systems affecting marginalized communities.

**Where to find live debate:** See [Issue #528](https://github.com/Trusted-AI/AIF360/issues/528) — "`average_odds_difference` metric is wrongly represented as an equalized odds relaxation" — a mathematical dispute about whether the toolkit's own documentation falsely equates a metric with a fairness criterion it does not actually satisfy.

---

## 2. [Aequitas](https://github.com/dssg/aequitas)

| | |
|---|---|
| **Owner** | David B. Lawrence III (dssg) |
| **Stars** | ⭐ 773 |
| **Language** | Python |
| **License** | MIT |
| **Last active** | Updated September 2026 |

**What it is:** A bias-auditing and fairness-ML toolkit designed to help data scientists detect, analyze, and mitigate bias in predictive models. It provides a systematic framework for generating fairness reports with group metrics, individual metrics, and intersectional analysis.

**Why it matters for social justice:** Aequitas emphasizes the audit trail — not just "is this model fair?" but "how do you prove it, and to whom?" Its intersectional analysis capabilities surface disparities that aggregate metrics can hide, which is crucial for communities that are multiply-marginalized.

**Open issues to watch:**
- [Issue #201](https://github.com/dssg/aequitas/issues/201) — "Add a readme or page on the existing metrics of fairness" — a documentation gap that makes it harder for non-experts to understand what each metric actually measures.
- [Issue #116](https://github.com/dssg/aequitas/issues/116) — "deal with multiclass problems" — fairness metrics are largely designed for binary classification; extending them to multiclass contexts (common in real-world scoring) is an open research challenge.

---

## 3. [Infosys Responsible AI Toolkit](https://github.com/Infosys/Infosys-Responsible-AI-Toolkit)

| | |
|---|---|
| **Owner** | Infosys |
| **Stars** | ⭐ 310 |
| **Language** | Python |
| **License** | Apache-2.0 |
| **Last active** | Updated September 2026 |

**What it is:** A broader responsible-AI toolkit that incorporates fairness and bias detection alongside security, explainability, and hallucination detection. It aims to provide end-to-end responsible-AI governance rather than focusing solely on fairness.

**Why it matters for social justice:** By bundling fairness with security and explainability, this toolkit reflects a growing recognition that bias doesn't exist in isolation — it intersects with transparency, accountability, and safety. For policy audiences, this holistic framing is practical: regulators and auditors need tools that address multiple harms simultaneously.

---

## How to Add a Project

1. Found a fairness/bias-auditing toolkit with active maintenance (last commit within 6 months)?
2. Check that it has a clear license and contribution guide.
3. Open a PR adding a new section with: name, owner, stars, language, 2-3 sentence description, "why it matters for social justice," and links to any active debate threads.
4. Tag the file `RESOURCES.md` in your PR title.
