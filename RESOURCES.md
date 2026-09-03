# AI Ethics Podcast — Open-Source Research Resources

A curated collection of active open-source projects in algorithmic fairness, bias auditing, and responsible ML — gathered for the **AI Ethics & Social Justice Podcast**. This repo is a **crowdsourced hub**: contributors are welcome to add projects, update descriptions, and flag new developments.

---

## Featured Projects

### 1. [Aequitas](https://github.com/dssg/aequitas) — *dssg / aequitas*
**Stars:** 772 | **Language:** Python | **License:** MIT

A widely-cited **bias auditing and Fair ML toolkit** from the University of Chicago Data Science public policy lab. Aequitas helps data scientists, ML researchers, and policymakers audit ML model predictions for bias across sensitive groups using confusion-matrix-based fairness metrics (TPR, FPR, PPV, etc.) and supports "correction" via Fair ML methods (pre-processing, in-processing, post-processing). It powers research on fairness trade-offs, includes an interactive API, and is cited in peer-reviewed publications.

**Why it matters for the podcast:** Aequitas operationalizes the sociotechnical challenges of fairness — it forces practitioners to choose *which* metrics matter for their domain and confronts the fact that no single fairness criterion can be satisfied simultaneously.

---

### 2. [FairML](https://github.com/asearer/FairML) — *asearer / FairML*
**Language:** Python | **CI/CD:** Active | **Test coverage:** 100%

A modular fairness-auditing and bias-injection toolkit with a **Streamlit dashboard** for interactive exploration of fairness metrics. FairML provides demographic parity difference, synthetic bias injection for experimentation, and a Dockerized deployment pipeline suitable for both research and production workflows. Its transformer-based architecture makes it adaptable to NLP and tabular use cases.

**Why it matters for the podcast:** FairML's hands-on, visual approach makes fairness tangible — listeners can explore how changing bias parameters shifts model behavior across groups, bridging the gap between abstract fairness metrics and lived experience.

---

### 3. [Fairlearn](https://github.com/fairlearn/fairlearn) — *Microsoft / fairlearn*
**Stars:** 2,284 | **Forks:** 514 | **Language:** Python | **License:** MIT

The most widely-adopted open-source fairness toolkit, backed by Microsoft research. Fairlearn offers both **assessment metrics** and **mitigation algorithms** (ExponentiatedGradient reductions, ThresholdOptimizer, etc.) spanning allocation harms and quality-of-service harms. It explicitly recognizes fairness as a *sociotechnical* challenge and directs users to the full context needed to make responsible trade-off decisions. Its Discord community and extensive documentation make it an entry point for practitioners.

**Why it matters for the podcast:** Fairlearn's own governance discussions — particularly around which fairness constraints to ship, how to name them, and whether mitigation tools are fit for purpose in domains like policing — mirror the real societal debates about AI ethics.

---

## How to Contribute

Got a project to add? Open a PR or issue with:
- Repository name and link
- Brief description (what it does, who it serves)
- Stars/forks/activity level
- Why it's relevant to algorithmic fairness and social justice
