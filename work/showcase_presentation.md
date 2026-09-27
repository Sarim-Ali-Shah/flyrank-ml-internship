# Week 8 ML-12: Presentation & Communication Assets
**Author:** Sarim Ali Shah  
**Project:** Evidence-Based Content Refresh & Decay Prevention  
**Lane:** Lane 2 — Refresh / Content Opportunity Scoring  
**Dataset:** FlyRank Production Search Dataset (79M rows across 32 clients)  
**Deployed Paper:** [https://sarim-ali-shah.github.io/flyrank-ml-internship/](https://sarim-ali-shah.github.io/flyrank-ml-internship/)  
**Repository:** [https://github.com/Sarim-Ali-Shah/flyrank-ml-internship](https://github.com/Sarim-Ali-Shah/flyrank-ml-internship)

---

## 1. 5-Minute Technical Showcase Demo Outline
Structured specifically around the 5-part rubric: **Question → Method → One Chart → One Honest Result → One Recommendation**.

### Minute 1: The Question (The Real FlyRank Problem)
- **Operational Reality:** FlyRank monitors organic search performance across millions of URLs for enterprise tenants. As search engines refresh ranking models and competitors publish new content, high-performing URLs experience silent decay (gradual loss of impressions, position, and clicks).
- **The Editorial Bottleneck:** Enterprise catalogs contain 10,000 to 50,000+ pages, but human editorial teams can realistically review and refresh only 20 to 50 articles per sprint. Naive "oldest first" rules waste time on zero-traffic ghost pages, while automated LLM rewrites risk brand voice and hallucination on key money pages.
- **The Core Decision:** *Which visible, high-impact content URLs should an editorial team prioritize for refresh, expansion, or CTR remediation to prevent organic traffic decay?*

### Minute 2: The Method (Honest Validation Design & Leakage Defense)
- **Data Scope:** 79 million search console interactions sampled to 30,000 URLs across 32 clients, utilizing a 90-day lookback window ($W_{\text{lookback}}$) and a 90-day forward evaluation window ($W_{\text{forward}}$). Fully anonymized with synthetic tokens (`client_4e07408562`).
- **Validation Rigor:** Standard random row-level k-fold splits leak client domain authority, artificially inflating AUC to 0.6611. We enforce a strict `GroupShuffleSplit` on `client_id` (25 train clients, 7 held-out test clients), ensuring models are validated only on unseen client domains (true generalization AUC = 0.5911).
- **Feature Cleanliness:** Confirmed zero forward-window data leakage.

### Minute 3: One Chart (`work/figures/model_comparison.png`)
- **Key Visual:** Present the dual-panel model comparison chart showing Precision@K on the left and ROC-AUC on the right across the Baseline, Logistic Regression, Decision Tree, and Random Forest.
- **Visual Focus:** Point out how the shallow Decision Tree sustains 68.0% precision across the top-50 queue, well above the 51.1% test base rate line.

### Minute 4: One Honest Result (Decision Tree vs. Random Forest)
- **The Finding:** A shallow Decision Tree (depth=3) achieved **68.0% Precision@50** and **70.0% Precision@20** on unseen client sites, providing a **+16.9 percentage point lift** over the 51.1% random base rate.
- **Honest Limitations:** Complex models like Random Forest overfitted to high-volume outlier queries (P@20 dropped to 40.0%). We explicitly state that predictions are *observational decision-support*, not causal ranking discoveries. We do not claim to reverse-engineer Google's ranking algorithm.

### Minute 5: One Recommendation (The Content Action Playbook)
- **Operational Triage:** Model probabilities are mapped into 4 actionable archetypes:
  1. `ctr_and_refresh` (4,889 URLs, 16.3%): Page 1 decay risk (positions 4–10) -> Title/meta overhaul & freshness updates.
  2. `expand_and_refresh` (3,394 URLs, 11.3%): Striking distance (positions 11–20) -> Add depth and internal links.
  3. `defend_pillar` (1,828 URLs, 6.1%): Top 3 positions -> Defensive competitor monitoring.
  4. `monitor` (19,853 URLs, 66.2%): Deep tail -> Excluded from sprint queues.
- **Governance:** Strict NO-GO rules bar autonomous generative rewrites, automated canonical changes, or bulk page deletion.

---

## 2. Two Shareable Cuts of the Work

### Cut 1: Short Social Post (Methodology & Finding — LinkedIn / X Ready)
```text
Most content refresh workflows in SEO rely on naive heuristics: "update whatever is oldest" or run automated LLM rewrites across thousands of URLs. Both waste immense editorial resources.

Working with FlyRank's 79-million-row production search dataset, I built a machine learning decision-support framework to prioritize content remediation before organic decay takes hold:

Key findings & methodology:
- Naive validation leaks: A standard random split overstates AUC (0.6611) by memorizing client domain authority. A true grouped-client split reveals the honest baseline (0.5911).
- Decision Trees beat complex ensembles operationally: achieving 68.0% Precision@50 on held-out client websites (vs a 51.1% base rate), while preserving interpretable rule structures for editors.
- Actionable Archetypes: We translate raw probabilities into a structured playbook (CTR fixes, striking-distance expansion, pillar defense) with a strict human-in-the-loop review policy.

Full research paper, validation audit, and code: https://sarim-ali-shah.github.io/flyrank-ml-internship/
Data courtesy of https://flyrank.ai/
```

### Cut 2: 3-Sentence Employer-Facing Summary ("What I built, on what data, what it showed")
> *Engineered an evidence-based content opportunity and decay ranking pipeline evaluated on 79M search events across 32 enterprise tenants, deploying a Decision Tree framework that achieved 68.0% Precision@50 on unseen client domains (a +16.9pp lift over base rate).*  
> *Enforced rigorous grouped-client validation and leakage audits to eliminate domain-memorization bias, replacing arbitrary editorial guesswork with statistically validated priority scores.*  
> *Delivered an end-to-end operational playbook pairing machine learning rankings with human-in-the-loop reason codes, published as a fully reproducible, public-safe research paper.*
